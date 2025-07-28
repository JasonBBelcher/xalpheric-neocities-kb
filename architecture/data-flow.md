# Data Flow Architecture

## Overview

The Xalpheric Neocities system implements multiple **parallel data processing pipelines** that converge into a unified deployment system. Each pipeline handles specific content types (text, images, audio) with optimized processing workflows.

## Primary Data Flows

### 1. Content Publishing Flow

```xml
<content-flow>
  <input>thoughts-and-musings/*.md</input>
  <processor>build-musings.js</processor>
  <transformation>
    <step>Read markdown files from source directory</step>
    <step>Sort by modification time (newest first)</step>
    <step>Sanitize filenames for web-safe URLs</step>
    <step>Convert markdown to HTML using markdown-it</step>
    <step>Wrap in HTML template with navigation</step>
    <step>Generate index.html with article links</step>
  </transformation>
  <output>public/musings/*.html + public/musings/index.html</output>
  <deployment>deploy-musings.js</deployment>
</content-flow>
```

**Detailed Process**:
1. **File Discovery**: Scan `thoughts-and-musings/` for `.md` files
2. **Metadata Extraction**: Get file modification times for sorting
3. **Content Processing**: For each markdown file:
   - Extract title from filename
   - Sanitize title: `My Post Title.md` → `My-Post-Title.html`
   - Convert markdown content to HTML
   - Inject into template with CSS/JS includes
   - Write to `public/musings/`
4. **Index Generation**: Create navigation page with chronological links
5. **Upload Preparation**: Add all generated files to upload queue

### 2. Image Processing Flow

```xml
<image-flow>
  <input>process_photos/ (HEIC, JPEG, PNG)</input>
  <processor>process-photos.js + ImageMagick</processor>
  <transformation>
    <step>Validate ImageMagick dependency</step>
    <step>Parse command-line arguments (size, format, naming)</step>
    <step>Discover input images in workspace</step>
    <step>Process each image with ImageMagick</step>
    <step>Apply naming patterns if specified</step>
    <step>Move processed images to public/assets/</step>
    <step>Clean up temporary files</step>
  </transformation>
  <output>public/assets/ (web-optimized images)</output>
  <integration>Gallery system references via gallery.js</integration>
</image-flow>
```

**Processing Parameters**:
- **Size**: `512x512`, `1024x768`, etc. (maintains aspect ratio)
- **Format**: `jpg`, `png` (converts HEIC automatically)
- **Naming**: Original names or patterns like `studio{increment}`

**Example Command Flow**:
```bash
npm run process-photos -- 512x512 jpg studio{increment}
# Results in: studio1.jpg, studio2.jpg, studio3.jpg
```

### 3. Video-to-Audio Conversion Flow

```xml
<video-flow>
  <input>process_video/ (MP4, MOV, M4V)</input>
  <processor>process-video.js + FFmpeg</processor>
  <configuration>JSON mapping: [{inputName, outputName}]</configuration>
  <transformation>
    <step>Validate FFmpeg and jq dependencies</step>
    <step>Parse JSON configuration from command line</step>
    <step>Validate input files exist</step>
    <step>Process each video with FFmpeg audio extraction</step>
    <step>Move audio files to public/music/</step>
    <step>Report processing results</step>
  </transformation>
  <output>public/music/ (MP3, WAV audio files)</output>
  <integration>Audio player references via main.js</integration>
</video-flow>
```

**Configuration Example**:
```json
[
  {"inputName": "live-performance.MOV", "outputName": "live-track-01.mp3"},
  {"inputName": "studio-session.mp4", "outputName": "demo-track.wav"}
]
```

## Deployment Data Flows

### 1. Incremental Deployment (deploy-musings.js)

```xml
<incremental-deployment>
  <trigger>npm run deploy</trigger>
  <scope>musings content only</scope>
  <process>
    <step>Check Node.js dependencies</step>
    <step>Validate NEOCITIES_API_KEY</step>
    <step>Build musings from markdown (build-musings.js)</step>
    <step>Collect all files in public/musings/</step>
    <step>Upload each file with retry logic</step>
    <step>Report success/failure statistics</step>
  </process>
  <optimization>Fast turnaround for content updates</optimization>
</incremental-deployment>
```

### 2. Full Site Refresh (deploy-full-refresh.js)

```xml
<full-deployment>
  <trigger>npm run deploy-full-refresh [options]</trigger>
  <scope>entire public/ directory</scope>
  <process>
    <step>Parse command-line options</step>
    <step>Build musings to ensure latest content</step>
    <step>Scan local files in public/</step>
    <step>Fetch remote file list from Neocities API</step>
    <step>Compare SHA1 hashes to identify changes</step>
    <step>Delete removed files from remote</step>
    <step>Upload new/changed files with rate limiting</step>
    <step>Report comprehensive deployment statistics</step>
  </process>
  <optimization>SHA1 comparison prevents unnecessary uploads</optimization>
</full-deployment>
```

**Options Impact on Data Flow**:
- `--dry-run`: Stop before actual upload, report what would happen
- `--include-mp3s`: Include audio files (otherwise skipped)
- `--include-assets`: Include assets directory (otherwise skipped)

### 3. Asset-Only Deployment (deploy-assets.js)

```xml
<asset-deployment>
  <trigger>GitHub Actions or manual execution</trigger>
  <scope>public/assets/ directory only</scope>
  <process>
    <step>Scan local assets directory</step>
    <step>Calculate SHA1 hashes for change detection</step>
    <step>Fetch remote assets list via API</step>
    <step>Compare hashes to identify new/changed files</step>
    <step>Upload only changed assets</step>
    <step>Report upload results</step>
  </process>
  <optimization>Specialized for CI/CD media updates</optimization>
</asset-deployment>
```

## Data Transformation Details

### Markdown to HTML Transformation

```xml
<markdown-transform>
  <input-format>
    <structure>
      # Title
      
      Content with **formatting**
      
      - Lists
      - Links
      - Code blocks
    </structure>
  </input-format>
  
  <processing-pipeline>
    <step name="markdown-it">Convert MD to HTML</step>
    <step name="template-wrap">Inject into HTML template</step>
    <step name="metadata-add">Add title, CSS, JS includes</step>
    <step name="navigation-inject">Add back-to-musings link</step>
  </processing-pipeline>
  
  <output-format>
    <structure>
      <!DOCTYPE html>
      <html>
        <head>
          <title>Xalpheric - {title}</title>
          <link rel="stylesheet" href="../css/theme.css">
          <!-- Additional meta and includes -->
        </head>
        <body>
          <nav class="back-nav">
            <a href="../musings.html">Back to Musings</a>
          </nav>
          <section class="musings-container">
            <div class="note-content">
              {converted-markdown-content}
            </div>
          </section>
        </body>
      </html>
    </structure>
  </output-format>
</markdown-transform>
```

### Image Processing Transformation

```xml
<image-transform>
  <input-formats>HEIC, JPEG, PNG, AVIF</input-formats>
  <imagemagick-command>
    magick input.heic -resize {width}x{height}> -quality 85 output.jpg
  </imagemagick-command>
  <optimizations>
    <resize>Maintains aspect ratio with > operator</resize>
    <quality>85% JPEG quality for web optimization</quality>
    <format>Automatic HEIC to JPEG conversion</format>
  </optimizations>
  <naming-logic>
    <original>Keep source filename</original>
    <pattern>Replace with studio1.jpg, studio2.jpg, etc.</pattern>
    <increment>Auto-increment for sequential naming</increment>
  </naming-logic>
</image-transform>
```

### Video to Audio Extraction

```xml
<video-transform>
  <ffmpeg-command>
    ffmpeg -i input.mp4 -vn -acodec libmp3lame -ab 192k output.mp3
  </ffmpeg-command>
  <parameters>
    <video-disable>-vn (no video stream)</video-disable>
    <audio-codec>libmp3lame for MP3 output</audio-codec>
    <bitrate>192k for good quality/size balance</bitrate>
  </parameters>
  <json-config>
    <structure>[{"inputName": "source.mp4", "outputName": "result.mp3"}]</structure>
    <validation>Checks file existence before processing</validation>
  </json-config>
</video-transform>
```

## Error Handling and Recovery

### Retry Logic Flow

```xml
<error-handling>
  <network-errors>
    <retry-count>3 attempts maximum</retry-count>
    <backoff-strategy>Progressive delay: 1s, 2s, 3s</backoff-strategy>
    <failure-action>Log error and continue with next file</failure-action>
  </network-errors>
  
  <api-errors>
    <rate-limiting>2-second delay between uploads</rate-limiting>
    <auth-errors>Stop immediately, check API key</auth-errors>
    <quota-errors>Report and suggest solutions</quota-errors>
  </api-errors>
  
  <processing-errors>
    <dependency-missing>Auto-install prompts or exit with instructions</dependency-missing>
    <file-errors>Skip problematic files, continue processing</file-errors>
    <permission-errors>Clear error messages with resolution steps</permission-errors>
  </processing-errors>
</error-handling>
```

## Performance Optimization Points

### 1. Parallel Processing Opportunities
- Image processing can be parallelized (currently sequential)
- File uploads could be parallelized with connection pooling
- Dependency checking could be cached

### 2. Caching Strategies
- SHA1 hash caching for large files
- Processed image caching to avoid reprocessing
- API response caching for file listings

### 3. Bandwidth Optimization
- Progressive JPEG for images
- Selective deployment based on file changes
- Compression for text files

## Data Integrity Measures

### File Verification
- SHA1 hash comparison for upload verification
- File size validation during processing
- Existence checks before operations

### Rollback Capabilities
- Git version control for source files
- Neocities file listing for remote state verification
- Dry-run mode for testing changes

### Backup Considerations
- Source files remain in thoughts-and-musings/
- Processing workspaces are temporary
- Generated files can be recreated from source

---

*This data flow architecture ensures reliable, efficient processing of all content types while maintaining system integrity and performance.*
