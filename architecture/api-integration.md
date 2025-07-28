# API Integration

## Neocities API Overview

The Xalpheric project integrates extensively with the **Neocities REST API** for file management and deployment. All deployment scripts use this API to upload, delete, and manage files on the Neocities hosting platform.

## API Configuration

### Authentication
```xml
<authentication>
  <method>Bearer Token</method>
  <header>Authorization: Bearer {NEOCITIES_API_KEY}</header>
  <storage>Environment variable NEOCITIES_API_KEY</storage>
  <security>Never hardcoded, loaded from .env or system environment</security>
</authentication>
```

### Base API Details
- **Base URL**: `https://neocities.org/api/`
- **Protocol**: HTTPS only
- **Content-Type**: `multipart/form-data` for uploads
- **Rate Limiting**: Implemented client-side with delays

## Core API Endpoints Used

### 1. File Upload Endpoint

```xml
<upload-endpoint>
  <url>POST https://neocities.org/api/upload</url>
  <purpose>Upload single or multiple files</purpose>
  <request-format>
    <method>POST</method>
    <headers>
      <authorization>Bearer {API_KEY}</authorization>
      <content-type>multipart/form-data</content-type>
    </headers>
    <body>
      <field name="{relative_path}">{file_content}</field>
      <!-- Multiple files can be included -->
    </body>
  </request-format>
  <response-format>
    <success>{"result": "success"}</success>
    <error>{"result": "error", "message": "error description"}</error>
  </response-format>
</upload-endpoint>
```

**Implementation Example from deploy-musings.js**:
```javascript
const form = new FormData();
form.append(relativePath, fs.createReadStream(filePath));

const options = {
  method: 'POST',
  host: 'neocities.org',
  path: '/api/upload',
  headers: {
    Authorization: `Bearer ${API_KEY}`,
    ...form.getHeaders()
  }
};
```

### 2. File Listing Endpoint

```xml
<list-endpoint>
  <url>GET https://neocities.org/api/list</url>
  <purpose>Retrieve all files and directories on the site</purpose>
  <request-format>
    <method>GET</method>
    <headers>
      <authorization>Bearer {API_KEY}</authorization>
    </headers>
  </request-format>
  <response-format>
    <structure>
      {
        "result": "success",  
        "files": [
          {
            "name": "filename.html",
            "is_directory": false,
            "size": 1024,
            "updated_at": "2025-01-01T12:00:00Z",
            "sha1_hash": "abc123...",
            "path": "relative/path/filename.html"
          }
        ]
      }
    </structure>
  </response-format>
</list-endpoint>
```

### 3. File Deletion Endpoint

```xml
<delete-endpoint>
  <url>POST https://neocities.org/api/delete</url>
  <purpose>Delete one or more files from the site</purpose>
  <request-format>
    <method>POST</method>
    <headers>
      <authorization>Bearer {API_KEY}</authorization>
      <content-type>application/x-www-form-urlencoded</content-type>
    </headers>
    <body>filenames[]={path1}&filenames[]={path2}</body>
  </request-format>
  <batch-limit>Maximum ~50 files per request</batch-limit>
</delete-endpoint>
```

## API Integration Patterns

### 1. Upload with Retry Logic

```xml
<retry-pattern>
  <implementation>Used in all deployment scripts</implementation>
  <max-attempts>3</max-attempts>
  <backoff-strategy>Progressive: 1s, 2s, 3s delays</backoff-strategy>
  <error-handling>
    <network-errors>Retry with backoff</network-errors>
    <api-errors>Log and continue with next file</api-errors>
    <auth-errors>Fail immediately, check API key</auth-errors>
  </error-handling>
</retry-pattern>
```

**Code Pattern**:
```javascript
async function uploadFile(filePath, retryCount = 0) {
  const maxRetries = 3;
  const retryDelay = 1000 * (retryCount + 1);
  
  try {
    // Upload logic here
    if (response.result !== 'success') {
      if (retryCount < maxRetries) {
        setTimeout(() => uploadFile(filePath, retryCount + 1), retryDelay);
      }
    }
  } catch (error) {
    // Network error handling
  }
}
```

### 2. Rate Limiting Implementation

```xml
<rate-limiting>
  <strategy>Client-side delay between requests</strategy>
  <delay>2000ms (2 seconds) between uploads</delay>
  <purpose>Respect API limitations and prevent throttling</purpose>
  <implementation>
    <location>All deployment scripts</location>
    <mechanism>setTimeout with Promise wrapper</mechanism>
  </implementation>
</rate-limiting>
```

**Implementation**:
```javascript
async function delay(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

// Between uploads
await delay(2000);
```

### 3. SHA1 Comparison for Efficiency

```xml
<sha1-optimization>
  <purpose>Avoid uploading unchanged files</purpose>
  <process>
    <step>Calculate local file SHA1 hash</step>
    <step>Fetch remote file list with hashes</step>
    <step>Compare local vs remote SHA1</step>
    <step>Upload only if hashes differ or file is new</step>
  </process>
  <benefit>Significantly reduces upload time for large sites</benefit>
</sha1-optimization>
```

## Error Handling Strategies

### API Response Parsing

```xml
<response-handling>
  <json-parsing>
    <safe-parse>tryParseJSON() function handles malformed responses</safe-parse>
    <fallback>Default error object if parsing fails</fallback>
  </json-parsing>
  <error-categorization>
    <auth-errors>"Invalid API key" -> immediate failure</auth-errors>
    <rate-limit>"Too many requests" -> extended retry</rate-limit>
    <server-errors>5xx responses -> retry with backoff</server-errors>
    <client-errors>4xx responses -> log and skip file</client-errors>
  </error-categorization>
</response-handling>
```

### Network Error Recovery

```xml
<network-recovery>
  <connection-errors>
    <timeout>Default Node.js timeout handling</timeout>
    <dns-errors>Fail fast with clear error message</dns-errors>
    <connection-reset>Retry with progressive backoff</connection-reset>
  </connection-errors>
  <partial-failures>
    <strategy>Continue processing remaining files</strategy>
    <reporting>Track success/failure counts</reporting>
    <exit-code>Non-zero if any failures occurred</exit-code>
  </partial-failures>
</network-recovery>
```

## API Usage Patterns by Script

### deploy-musings.js
- **Primary Operation**: Upload musings HTML files
- **API Calls**: Upload only (no listing or deletion)
- **File Scope**: `public/musings/` directory
- **Optimization**: Direct upload without change detection
- **Rate Limiting**: 2-second delays between files

### deploy-full-refresh.js
- **Primary Operations**: List, delete, upload
- **API Calls**: 
  1. List remote files
  2. Delete removed files (batch)
  3. Upload new/changed files
- **File Scope**: Entire `public/` directory
- **Optimization**: SHA1 comparison for change detection
- **Special Features**: Dry-run mode, selective inclusion

### deploy-assets.js
- **Primary Operations**: List, upload
- **API Calls**:
  1. List remote assets
  2. Upload changed assets only
- **File Scope**: `public/assets/` directory only
- **Optimization**: Specialized for CI/CD asset updates
- **Integration**: GitHub Actions compatible

## Security Considerations

### API Key Management
```xml
<security-practices>
  <storage>
    <development>.env file (gitignored)</development>
    <production>Environment variables</production>
    <ci-cd>GitHub Secrets</ci-cd>
  </storage>
  <validation>
    <presence-check>Scripts verify API key exists before proceeding</presence-check>
    <format-validation>Basic format checking</format-validation>
    <test-request>API key validated with actual API call</test-request>
  </validation>
  <error-handling>
    <invalid-key>Clear error message with setup instructions</invalid-key>
    <missing-key>Exit with usage information</missing-key>
  </error-handling>
</security-practices>
```

### File Path Security
- **Sanitization**: Relative paths normalized to prevent traversal
- **Validation**: File existence checked before upload attempts
- **Encoding**: Proper URL encoding for special characters in filenames

## Performance Optimizations

### Batch Operations
- **File Deletion**: Up to 50 files per delete request
- **Upload Grouping**: Sequential uploads with rate limiting
- **Connection Reuse**: Single HTTPS connection per script run

### Change Detection
- **SHA1 Hashing**: Prevents unnecessary uploads
- **File Size Comparison**: Quick check before hash calculation
- **Timestamp Optimization**: Local modification time tracking

### Memory Management
- **Streaming Uploads**: Files streamed rather than loaded into memory
- **Progressive Processing**: Files processed one at a time
- **Cleanup**: Temporary objects released promptly

## Monitoring and Logging

### Upload Progress Tracking
```xml
<progress-monitoring>
  <file-level>
    <status>✅ Uploaded: filename.html</status>
    <errors>❌ Failed to upload: filename.html (reason)</errors>
    <retries>🔄 Retrying in 2000ms...</retries>
  </file-level>
  <summary-stats>
    <successful>✅ Successful uploads: 15</successful>
    <failed>❌ Failed uploads: 2</failed>
    <total>📋 Total files: 17</total>
  </summary-stats>
</progress-monitoring>
```

### Error Reporting
- **Detailed API Responses**: Full server response logged on errors
- **Network Diagnostics**: Connection error details
- **Retry Tracking**: Number of attempts and outcomes
- **Performance Metrics**: Upload speeds and timing

---

*This API integration ensures reliable, efficient communication with the Neocities platform while maintaining security and performance best practices.*
