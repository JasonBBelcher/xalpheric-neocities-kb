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
- [System Overview](architecture/system-overview.md) ✅
- [File Structure](architecture/file-structure.md) ✅
- [Data Flow](architecture/data-flow.md) ✅
- [API Integration](architecture/api-integration.md) ✅

### 🎯 Features
- [Content Management](features/content-management.md) ✅
- [Gallery System](features/gallery-system.md) ✅
- [Audio System](features/audio-system.md) ✅

### 📦 Deployment
- [Deployment Strategy](deployment/deployment-strategy.md) ✅

### 🎨 Media Processing
- [Photo Processing](media-processing/photo-processing.md) ✅
- [Video Processing](media-processing/video-processing.md) ✅

### 🔄 Workflows
- [Development Workflow](workflows/development-workflow.md) ✅
- [GitHub Actions](workflows/github-actions.md) ✅

### 🔧 Dependencies
- [Dependency Management](dependencies/dependency-management.md) ✅

### 📚 Additional Resources
- [AI Context Prompt](AI-CONTEXT-PROMPT.md) - Instructions for AI assistants
- [Human Guide](HUMAN-GUIDE.md) - User-friendly project overview

## Usage Guidelines for AI Context

### Best Practices for Claude AI
1. **Long Context Optimization**: Documents are structured with essential information at the top
2. **XML-Style Organization**: Content uses clear hierarchical structures
3. **Quote-Friendly Format**: Key concepts are easily extractable for grounding responses
4. **Modular Design**: Each document focuses on a specific domain area

### Context Loading Strategy
- Start with `architecture/system-overview.md` for general understanding
- Use `features/content-management.md` and `features/gallery-system.md` for feature-specific questions
- Reference `deployment/deployment-strategy.md` for CI/CD and build processes
- Consult `workflows/development-workflow.md` for user journey understanding
- Check `media-processing/` documents for photo and video processing workflows
- Use `AI-CONTEXT-PROMPT.md` for optimal AI assistant interaction

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

### Key Directories (Main Project)
- `thoughts-and-musings/` - Source markdown content
- `public/` - Built website files
- `process_photos/` - Photo processing workspace
- `process_video/` - Video processing workspace
- `.github/scripts/` - Deployment and build scripts

### Important Files (Main Project)
- `build-musings.js` - Markdown to HTML converter
- `package.json` - Project configuration and scripts
- `public/js/gallery.js` - Gallery and lightbox functionality
- `public/css/theme.css` - Complete site theming

### Knowledge Base Files
- `AI-CONTEXT-PROMPT.md` - AI assistant context loading instructions
- `HUMAN-GUIDE.md` - Human-readable project overview
- `architecture/` - System architecture documentation
- `features/` - Feature-specific documentation
- `deployment/` - Deployment process documentation
- `media-processing/` - Photo and video processing guides
- `workflows/` - Development workflow documentation
- `dependencies/` - Dependency management information

## Maintenance Notes

This knowledge base should be updated when:
- New features are added to the project
- Deployment processes change
- Dependencies are updated
- Architecture modifications are made

### Future Documentation Areas
The following documentation sections could be expanded:
- `features/navigation-system.md` - Site navigation implementation
- `deployment/scripts-overview.md` - Detailed script documentation
- `deployment/rate-limiting.md` - API rate limiting strategies
- `deployment/error-handling.md` - Error handling patterns
- `media-processing/file-management.md` - File organization strategies
- `frontend/` directory - Frontend architecture documentation
- `workflows/cicd-pipeline.md` - Advanced CI/CD pipeline documentation
- `dependencies/system-dependencies.md` - System requirements
- `dependencies/nodejs-packages.md` - Node.js package documentation
- `security/api-key-management.md` - Security best practices
- `monitoring/performance-tracking.md` - Performance monitoring strategies

## AI Integration Notes

This knowledge base is optimized for Claude AI's 200K token context window and follows Anthropic's best practices for:
- Long document processing
- Multi-document analysis
- Structured information retrieval
- Context-aware responses

---

*Last Updated: July 28, 2025*
*Knowledge Base Version: 2.0 - Major updates including GitHub Actions, JSON configuration system, and audio system optimization*
