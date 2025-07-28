# Xalpheric Neocities Project Knowledge Base

## Overview

This knowledge base provides comprehensive documentation for the Xalpheric Neocities project - a complete static site builder and deployment system for the Neocities platform. The project features automated content management, media processing, and deployment workflows optimized for a music artist's website.

## Project Metadata

- **Project Name**: xalpheric-neocities
- **Version**: 1.0.0
- **Main Language**: JavaScript (Node.js)
- **Platform**: Neocities
- **Author**: Jason Belcher (Xalpheric)
- **License**: MIT

## Knowledge Base Structure

This knowledge base is organized following Claude AI context management best practices, with content structured for optimal AI understanding and retrieval:

### 📁 Architecture
- [System Overview](architecture/system-overview.md)
- [File Structure](architecture/file-structure.md)
- [Data Flow](architecture/data-flow.md)
- [API Integration](architecture/api-integration.md)

### 🎯 Features
- [Content Management](features/content-management.md)
- [Gallery System](features/gallery-system.md)
- [Audio Player](features/audio-player.md)
- [Navigation System](features/navigation-system.md)

### 📦 Deployment
- [Deployment Strategy](deployment/deployment-strategy.md)
- [Scripts Overview](deployment/scripts-overview.md)
- [Rate Limiting](deployment/rate-limiting.md)
- [Error Handling](deployment/error-handling.md)

### 🎨 Media Processing
- [Photo Processing](media-processing/photo-processing.md)
- [Video Processing](media-processing/video-processing.md)
- [File Management](media-processing/file-management.md)

### 🌐 Frontend
- [HTML Structure](frontend/html-structure.md)
- [CSS Theming](frontend/css-theming.md)
- [JavaScript Components](frontend/javascript-components.md)
- [Responsive Design](frontend/responsive-design.md)

### 🔄 Workflows
- [Development Workflow](workflows/development-workflow.md)
- [GitHub Actions](workflows/github-actions.md)
- [CI/CD Pipeline](workflows/cicd-pipeline.md)

### 🔧 Dependencies
- [System Dependencies](dependencies/system-dependencies.md)
- [Node.js Packages](dependencies/nodejs-packages.md)
- [Dependency Management](dependencies/dependency-management.md)

## Usage Guidelines for AI Context

### Best Practices for Claude AI
1. **Long Context Optimization**: Documents are structured with essential information at the top
2. **XML-Style Organization**: Content uses clear hierarchical structures
3. **Quote-Friendly Format**: Key concepts are easily extractable for grounding responses
4. **Modular Design**: Each document focuses on a specific domain area

### Context Loading Strategy
- Start with `architecture/system-overview.md` for general understanding
- Use feature-specific documents for targeted questions
- Reference deployment documents for CI/CD and build processes
- Consult workflow documents for user journey understanding

## Quick Reference

### Core Commands
```bash
# Build and deploy
npm run deploy

# Process media
npm run process-photos -- 512x512 jpg studio{increment}
npm run process-video -- '[{"inputName":"video.mp4","outputName":"audio.mp3"}]'

# Full site refresh
npm run deploy-full-refresh

# Development server
npm run serve
```

### Key Directories
- `thoughts-and-musings/` - Source markdown content
- `public/` - Built website files
- `process_photos/` - Photo processing workspace
- `process_video/` - Video processing workspace
- `.github/scripts/` - Deployment and build scripts

### Important Files
- `build-musings.js` - Markdown to HTML converter
- `package.json` - Project configuration and scripts
- `public/js/gallery.js` - Gallery and lightbox functionality
- `public/css/theme.css` - Complete site theming

## Maintenance Notes

This knowledge base should be updated when:
- New features are added to the project
- Deployment processes change
- Dependencies are updated
- Architecture modifications are made

## AI Integration Notes

This knowledge base is optimized for Claude AI's 200K token context window and follows Anthropic's best practices for:
- Long document processing
- Multi-document analysis
- Structured information retrieval
- Context-aware responses

---

*Last Updated: July 28, 2025*
*Knowledge Base Version: 1.0*
