# System Overview

## Executive Summary

The Xalpheric Neocities project is a **comprehensive static site builder and deployment system** designed specifically for the Neocities platform. It serves as a complete content management solution for a music artist's website, featuring automated markdown-to-HTML conversion, sophisticated media processing capabilities, and intelligent deployment workflows.

## Core Architecture

### High-Level System Design

```xml
<system-architecture>
  <content-layer>
    <source>thoughts-and-musings/*.md</source>
    <processor>build-musings.js</processor>
    <output>public/musings/*.html</output>
  </content-layer>
  
  <media-layer>
    <photo-processing>
      <input>process_photos/</input>
      <processor>process-photos.js</processor>
      <output>public/assets/</output>
    </photo-processing>
    <video-processing>
      <input>process_video/</input>
      <processor>process-video.js</processor>
      <output>public/music/</output>
    </video-processing>
  </media-layer>
  
  <deployment-layer>
    <incremental>.github/scripts/deploy-musings.js</incremental>
    <full-refresh>.github/scripts/deploy-full-refresh.js</full-refresh>
    <assets-only>.github/scripts/deploy-assets.js</assets-only>
  </deployment-layer>
  
  <frontend-layer>
    <static-files>public/</static-files>
    <javascript>public/js/</javascript>
    <stylesheets>public/css/</stylesheets>
  </frontend-layer>
</system-architecture>
```

## Technology Stack

### Core Technologies
- **Runtime**: Node.js (v14+)
- **Markdown Processing**: markdown-it
- **File Upload**: form-data
- **Environment Management**: dotenv
- **File Watching**: chokidar

### External Dependencies
- **Image Processing**: ImageMagick (magick command)
- **Video Processing**: FFmpeg
- **JSON Processing**: jq
- **Development Server**: http-server

### Platform Integration
- **Deployment Target**: Neocities.org
- **API**: Neocities REST API
- **CI/CD**: GitHub Actions
- **Version Control**: Git

## System Components

### 1. Content Management System

**Purpose**: Convert markdown files to styled HTML pages

**Key Files**:
- `build-musings.js` - Main conversion engine
- `thoughts-and-musings/` - Source markdown directory
- `public/musings/` - Generated HTML output

**Features**:
- Automatic HTML template wrapping
- Index page generation with navigation
- File sanitization and URL-safe naming
- Chronological sorting by modification time

### 2. Media Processing Pipeline

**Photo Processing**:
- Batch image resizing and format conversion
- HEIC support for iPhone photos
- Custom naming patterns (studio{increment}, etc.)
- Automatic web optimization

**Video Processing**:
- Video-to-audio conversion
- Flexible JSON configuration
- Multiple format support (MP4/MOV → MP3/WAV)
- Batch processing capabilities

### 3. Deployment System

**Incremental Deployment** (`deploy-musings.js`):
- Builds and deploys only musings content
- Optimized for content updates
- Fast turnaround for blog posts

**Full Site Refresh** (`deploy-full-refresh.js`):
- Complete site rebuild and upload
- SHA1 comparison to avoid unnecessary uploads
- Configurable file exclusions (MP3s, assets)
- Dry-run capability for safe testing

**Asset-Only Deployment** (`deploy-assets.js`):
- Specialized for media file updates
- GitHub Actions integration
- Automatic change detection

### 4. Frontend Architecture

**Static Site Structure**:
- Multi-page navigation system
- Responsive design with mobile optimization
- Gallery with lightbox functionality
- Custom audio player integration

**JavaScript Components**:
- Gallery management with image metadata
- Lightbox with keyboard navigation
- Audio player with playlist support
- Radio player functionality

**CSS Architecture**:
- Cyberpunk/glitch aesthetic theme
- Animated backgrounds and effects
- Responsive grid layouts
- Custom component styling

## Data Flow

### Content Publishing Flow
```
1. Write markdown in thoughts-and-musings/
2. Run `npm run deploy`
3. build-musings.js converts MD → HTML
4. deploy-musings.js uploads to Neocities
5. Site updates automatically visible
```

### Media Processing Flow
```
1. Place media in process_photos/ or process_video/
2. Run processing scripts with parameters
3. Files processed and moved to public/assets/ or public/music/
4. Optional: Update gallery.js configuration
5. Deploy via standard deployment process
```

### Development Flow
```
1. Local development with `npm run serve`
2. Test changes on localhost:8080
3. Process media as needed
4. Deploy incrementally or full refresh
5. Monitor via GitHub Actions (if configured)
```

## Key Design Principles

### 1. Modularity
- Each component handles a specific responsibility
- Scripts can be run independently or as part of workflows
- Clear separation between content, media, and deployment

### 2. Reliability
- Comprehensive error handling and retry logic
- Rate limiting to respect API constraints
- Dependency checking with auto-installation prompts
- Dry-run capabilities for safe testing

### 3. Performance
- Incremental deployment reduces unnecessary uploads
- SHA1 comparison prevents redundant transfers
- Optimized media processing with batch operations
- Efficient file structure for fast loading

### 4. User Experience
- Simple npm script interface
- Interactive dependency installation
- Detailed logging and progress feedback
- Flexible configuration options

## Security Considerations

### API Key Management
- Environment variable storage (`NEOCITIES_API_KEY`)
- GitHub Secrets integration for CI/CD
- No hardcoded credentials in source code

### File Processing
- Input validation for media processing
- Safe filename sanitization
- Directory traversal protection
- File type validation

### Deployment Safety
- Dry-run mode for testing
- Upload verification and retry logic
- Rate limiting to prevent API abuse
- Rollback capabilities through version control

## Scalability and Maintenance

### Current Limitations
- Single-user design (not multi-tenant)
- Manual gallery configuration updates
- No content versioning beyond Git
- Limited to Neocities platform

### Future Enhancement Opportunities
- Automated gallery metadata generation
- Content scheduling and publishing workflows
- Multi-site deployment support
- Enhanced media optimization options

## Performance Characteristics

### Build Times
- Musings build: ~1-5 seconds for typical content
- Photo processing: ~5-15 seconds per image
- Video processing: ~30-60 seconds per video
- Full deployment: ~2-10 minutes depending on content size

### Resource Usage
- Low memory footprint (~50-100MB during processing)
- CPU-intensive during media processing
- Network-bound during deployment
- Minimal storage requirements

---

*This overview provides the foundation for understanding all other components in the knowledge base.*
