# Development Workflow

## Overview

The Xalpheric development workflow is designed for **efficient content creation, media processing, and deployment** with minimal friction between creative work and technical implementation. The workflow supports multiple development scenarios from quick content updates to comprehensive site changes.

## Core Development Patterns

### Primary Workflow Types
```xml
<workflow-types>
  <content-focused-workflow>
    <purpose>Blog posts, musings, text content updates</purpose>
    <frequency>Regular, multiple times per week</frequency>
    <tools>Markdown editor, text editor</tools>
    <deployment>Incremental (deploy-musings.js)</deployment>
    <duration>~5-15 minutes from creation to live</duration>
  </content-focused-workflow>
  
  <media-focused-workflow>
    <purpose>Gallery updates, photo processing, audio additions</purpose>
    <frequency>Periodic, after photo sessions or events</frequency>
    <tools>Photo processing, video conversion</tools>
    <deployment>Asset-specific or full refresh</deployment>
    <duration>~15-60 minutes depending on media volume</duration>
  </media-focused-workflow>
  
  <site-development-workflow>
    <purpose>Feature additions, styling changes, functionality updates</purpose>
    <frequency>Occasional, major improvements</frequency>
    <tools>Code editor, browser dev tools, local server</tools>
    <deployment>Full site refresh</deployment>
    <duration>~1-4 hours for typical features</duration>
  </site-development-workflow>
  
  <maintenance-workflow>
    <purpose>Dependency updates, system maintenance, troubleshooting</purpose>
    <frequency>As needed, monthly reviews</frequency>
    <tools>Terminal, package managers, monitoring tools</tools>
    <deployment>Testing and validation focused</deployment>
    <duration>~30 minutes to 2 hours</duration>
  </maintenance-workflow>
</workflow-types>
```

## Content Creation Workflow

### Typical Content Publishing Process
```xml
<content-workflow>
  <creation-phase>
    <step>Identify topic or subject for new content</step>
    <step>Create new markdown file in thoughts-and-musings/</step>
    <step>Use descriptive filename: "My New Post Topic.md"</step>
    <step>Write content using standard markdown formatting</step>
    <step>Save file (modification time becomes publication order)</step>
  </creation-phase>
  
  <local-testing-phase>
    <step>Optional: Run npm run build-musings to test locally</step>
    <step>Optional: Start local server with npm run serve</step>
    <step>Optional: Review generated HTML at localhost:8080</step>
    <step>Make corrections to markdown if needed</step>
  </local-testing-phase>
  
  <deployment-phase>
    <step>Run npm run deploy for quick content deployment</step>
    <step>Monitor deployment progress and success/failure</step>
    <step>Verify content appears correctly on live site</step>
    <step>Share or promote new content as appropriate</step>
  </deployment-phase>
  
  <post-publication>
    <step>Monitor for any issues or feedback</step>
    <step>Make corrections if needed and redeploy</step>
    <step>Consider linking from other content or social media</step>
  </post-publication>
</content-workflow>
```

### Content Organization Strategies
```xml
<content-organization>
  <filename-conventions>
    <descriptive-names>Use clear, descriptive filenames</descriptive-names>
    <avoid-special-chars>Stick to letters, numbers, spaces, and hyphens</avoid-special-chars>
    <version-control>Use Git for drafts and revision history</version-control>
    <examples>
      <good>"My Studio Setup Evolution.md"</good>
      <good>"Learning Modular Synthesis.md"</good>
      <avoid>"post1.md", "temp-file.md"</avoid>
    </examples>
  </filename-conventions>
  
  <content-planning>
    <draft-system>
      <git-branches>Use feature branches for work-in-progress</git-branches>
      <draft-folder>Could use separate draft directory</draft-folder>
      <filename-prefixes>Use "DRAFT-" prefix for unpublished content</filename-prefixes>
    </draft-system>
    
    <series-management>
      <numbering>Use consistent numbering for multi-part content</numbering>
      <cross-references>Link between related posts in markdown</cross-references>
      <tagging>Could implement tag system in future</tagging>
    </series-management>
  </content-planning>
</content-organization>
```

## Media Processing Workflow

### Photo Processing Workflow
```xml
<photo-workflow>
  <preparation-phase>
    <step>Gather photos from camera, phone, or other sources</step>
    <step>Review and select best images for web publication</step>
    <step>Place selected images in process_photos/ directory</step>
    <step>Consider what size and naming pattern to use</step>
  </preparation-phase>
  
  <processing-phase>
    <step>Determine appropriate size (512x512 for thumbnails, 1024x768 for detailed viewing)</step>
    <step>Choose format (jpg for photos, png for graphics/screenshots)</step>
    <step>Decide on naming pattern (studio{increment}, event{increment}, or original names)</step>
    <step>Run processing: npm run process-photos -- 1024x768 jpg studio{increment}</step>
    <step>Verify processed images in public/assets/</step>
  </processing-phase>
  
  <gallery-integration>
    <step>Open public/js/gallery.js in editor</step>
    <step>Add new image objects to galleryImages array</step>
    <step>Include filename, displayName, and description</step>
    <step>Save gallery.js with updated configuration</step>
    <step>Test gallery locally if desired</step>
  </gallery-integration>
  
  <deployment-phase>
    <step>Deploy assets: npm run deploy-assets.js (or full refresh)</step>
    <step>Deploy gallery configuration: npm run deploy</step>
    <step>Verify images appear correctly in live gallery</step>
    <step>Test lightbox functionality and metadata display</step>
  </deployment-phase>
</photo-workflow>
```

### Video Processing Workflow
```xml
<video-workflow>
  <preparation-phase>
    <step>Collect video files (performance recordings, studio sessions)</step>
    <step>Place video files in process_video/ directory</step>
    <step>Plan output audio filenames and formats</step>
    <step>Create JSON configuration for input/output mapping</step>
  </preparation-phase>
  
  <configuration-creation>
    <step>Write JSON array with input/output pairs:
      [
        {"inputName": "live-session.MOV", "outputName": "track-01.mp3"},
        {"inputName": "studio-demo.mp4", "outputName": "demo-song.wav"}
      ]
    </step>
    <step>Validate JSON syntax (use online JSON validator if needed)</step>
    <step>Ensure input files match exactly (case-sensitive)</step>
  </configuration-creation>
  
  <processing-execution>
    <step>Run video processing with JSON config:</step>
    <step>npm run process-video -- '[{"inputName":"video.mp4","outputName":"audio.mp3"}]'</step>
    <step>Monitor processing progress and any error messages</step>
    <step>Verify output audio files in public/music/</step>
    <step>Test audio quality and format compatibility</step>
  </processing-execution>
  
  <audio-integration>
    <step>Update audio player configuration in public/js/main.js if needed</step>
    <step>Add new tracks to releases array or playlist</step>
    <step>Test audio player functionality locally</step>
    <step>Deploy updated audio files and configuration</step>
  </audio-integration>
</video-workflow>
```

## Development Environment Setup

### Local Development Environment
```xml
<local-development>
  <initial-setup>
    <step>Clone repository: git clone [repo-url]</step>
    <step>Navigate to project: cd xalpheric-neocities</step>
    <step>Install Node.js dependencies: npm install</step>
    <step>Check system dependencies: npm run check-deps</step>
    <step>Install missing system dependencies as prompted</step>
    <step>Create .env file with NEOCITIES_API_KEY</step>
  </initial-setup>
  
  <development-server>
    <command>npm run serve</command>
    <url>http://localhost:8080</url>
    <purpose>Preview changes before deployment</purpose>
    <features>
      <static-serving>Serves all files in public/ directory</static-serving>
      <live-reload>Manual refresh needed for changes</live-reload>
      <network-access>Accessible from other devices on network</network-access>
    </features>
  </development-server>
  
  <testing-workflow>
    <step>Make changes to source files (markdown, CSS, JS)</step>
    <step>Build updated content: npm run build-musings</step>
    <step>Test in browser at localhost:8080</step>
    <step>Iterate on changes as needed</step>
    <step>Deploy when satisfied: npm run deploy</step>
  </testing-workflow>
</local-development>
```

### Development Tools and Editor Setup
```xml
<development-tools>
  <recommended-editors>
    <vscode>
      <extensions>
        <markdown>Markdown preview and editing</markdown>
        <prettier>Code formatting</prettier>
        <eslint>JavaScript linting</eslint>
        <live-server>Local development server</live-server>
      </extensions>
    </vscode>
    <alternatives>
      <sublime-text>Lightweight, fast editing</sublime-text>
      <vim-neovim>Terminal-based editing</vim-neovim>
      <atom>GitHub's editor (deprecated but still usable)</atom>
    </alternatives>
  </recommended-editors>
  
  <browser-tools>
    <chrome-devtools>Inspect elements, debug JavaScript, test responsive design</chrome-devtools>
    <firefox-devtools>CSS grid inspector, accessibility tools</firefox-devtools>
    <extensions>
      <lighthouse>Performance and SEO auditing</lighthouse>
      <wave>Accessibility testing</wave>
    </extensions>
  </browser-tools>
  
  <terminal-setup>
    <shell-preferences>zsh with Oh My Zsh, bash, or fish</shell-preferences>
    <helpful-aliases>
      <build>alias build="npm run build-musings"</build>
      <serve>alias serve="npm run serve"</serve>
      <deploy>alias deploy="npm run deploy"</deploy>
    </helpful-aliases>
  </terminal-setup>
</development-tools>
```

## Git Workflow and Version Control

### Branch Strategy
```xml
<git-workflow>
  <branch-structure>
    <main-branch>
      <name>main</name>
      <purpose>Production-ready code</purpose>
      <deployment>Automatically deployed via CI/CD</deployment>
      <protection>Should be stable, tested changes only</protection>
    </main-branch>
    
    <feature-branches>
      <naming>feature/description-of-feature</naming>
      <purpose>Develop new features or major changes</purpose>
      <workflow>Create → develop → test → merge to main</workflow>
      <examples>
        <feature-gallery>feature/improved-gallery-system</feature-gallery>
        <feature-audio>feature/enhanced-audio-player</feature-audio>
      </examples>
    </feature-branches>
    
    <content-branches>
      <naming>content/post-topic</naming>
      <purpose>Draft long-form content before publication</purpose>
      <workflow>Write → review → merge when ready to publish</workflow>
      <optional>Simple content can be committed directly to main</optional>
    </content-branches>
  </branch-structure>
  
  <commit-practices>
    <message-format>
      <conventional>Use conventional commit format when possible</conventional>
      <examples>
        <feature>feat: add lightbox navigation with keyboard support</feature>
        <fix>fix: resolve mobile gallery layout issues</fix>
        <content>content: add new post about studio workflow</content>
        <docs>docs: update README with new deployment options</docs>
      </examples>
    </message-format>
    
    <commit-frequency>
      <small-commits>Make small, logical commits</small-commits>
      <working-state>Commit working state at end of sessions</working-state>
      <before-deploy>Always commit before deploying</before-deploy>
    </commit-frequency>
  </commit-practices>
</git-workflow>
```

### Deployment Integration with Git
```xml
<git-deployment-integration>
  <pre-deployment-checklist>
    <step>Commit all changes to Git</step>
    <step>Push to remote repository</step>
    <step>Verify working directory is clean</step>
    <step>Consider creating tag for major releases</step>
  </pre-deployment-checklist>
  
  <automated-deployment>
    <github-actions>
      <trigger>Push to main branch</trigger>
      <scope>Assets deployment only (currently)</scope>
      <expansion>Could be extended to full site deployment</expansion>
    </github-actions>
    
    <manual-deployment>
      <reason>More control over timing and scope</reason>
      <flexibility>Choose incremental vs full deployment</flexibility>
      <testing>Use dry-run mode for validation</testing>
    </manual-deployment>
  </automated-deployment>
  
  <rollback-strategies>
    <git-revert>Use git revert for problematic commits</git-revert>
    <branch-rollback>Deploy from previous stable branch/tag</branch-rollback>
    <selective-rollback>Revert specific files while keeping others</selective-rollback>
  </rollback-strategies>
</git-deployment-integration>
```

## Testing and Quality Assurance

### Testing Workflow
```xml
<testing-workflow>
  <local-testing>
    <content-testing>
      <markdown-preview>Preview markdown in editor</markdown-preview>
      <local-build>Build and serve locally before deployment</local-build>
      <link-checking>Verify internal and external links work</link-checking>
      <mobile-testing>Test responsive design on different screen sizes</mobile-testing>
    </content-testing>
    
    <functionality-testing>
      <gallery-testing>Test image loading, lightbox, navigation</gallery-testing>
      <audio-testing>Verify audio player works with new tracks</audio-testing>
      <navigation-testing>Test site navigation and breadcrumbs</navigation-testing>
      <form-testing>Test any interactive elements</form-testing>
    </functionality-testing>
  </local-testing>
  
  <deployment-testing>
    <dry-run-validation>
      <command>npm run deploy-full-refresh -- --dry-run</command>
      <purpose>Verify what changes would be deployed</purpose>
      <safety>No actual changes made to live site</safety>
    </dry-run-validation>
    
    <staged-deployment>
      <incremental-first>Deploy content changes incrementally</incremental-first>
      <verify-functionality>Test functionality after each deployment</verify-functionality>
      <full-refresh-sparingly>Use full refresh only when necessary</full-refresh-sparingly>
    </staged-deployment>
  </deployment-testing>
  
  <post-deployment-verification>
    <live-site-testing>Test functionality on live site</live-site-testing>
    <performance-checking>Verify page load speeds</performance-checking>
    <cross-browser-testing>Test in different browsers when possible</cross-browser-testing>
    <mobile-verification>Test on actual mobile devices</mobile-verification>
  </post-deployment-verification>
</testing-workflow>
```

### Quality Assurance Practices
```xml
<quality-assurance>
  <code-quality>
    <consistency>Follow established patterns and conventions</consistency>
    <documentation>Comment complex functionality</documentation>
    <error-handling>Include appropriate error handling</error-handling>
    <performance>Consider performance impact of changes</performance>
  </code-quality>
  
  <content-quality>
    <proofreading>Review content for typos and clarity</proofreading>
    <formatting>Ensure consistent markdown formatting</formatting>
    <accessibility>Use descriptive alt text for images</accessibility>
    <seo-considerations>Use descriptive titles and headings</seo-considerations>
  </content-quality>
  
  <user-experience>
    <navigation-clarity>Ensure clear site navigation</navigation-clarity>
    <loading-performance>Optimize image sizes and loading</loading-performance>
    <mobile-experience>Test and optimize mobile usability</mobile-experience>
    <accessibility>Consider screen readers and keyboard navigation</accessibility>
  </user-experience>
</quality-assurance>
```

## Troubleshooting and Debugging

### Common Development Issues
```xml
<troubleshooting>
  <dependency-issues>
    <symptoms>Scripts fail with "command not found" errors</symptoms>
    <diagnosis>Run npm run check-deps to identify missing dependencies</diagnosis>
    <resolution>Install missing system packages or Node modules</resolution>
    <prevention>Regularly run check-deps, especially after system updates</prevention>
  </dependency-issues>
  
  <deployment-failures>
    <symptoms>Upload errors, API failures, or partial deployments</symptoms>
    <diagnosis>Check API key, network connection, and Neocities status</diagnosis>
    <resolution>Retry deployment, check error messages, verify file permissions</resolution>
    <prevention>Use dry-run mode, maintain stable internet connection</prevention>
  </deployment-failures>
  
  <build-issues>
    <symptoms>Generated HTML looks wrong or content missing</symptoms>
    <diagnosis>Check markdown syntax, file permissions, output directory</diagnosis>
    <resolution>Fix markdown formatting, verify file paths, check console output</resolution>
    <prevention>Test locally before deployment, use consistent formatting</prevention>
  </build-issues>
  
  <media-processing-issues>
    <symptoms>Images not processing, wrong sizes, or missing files</symptoms>
    <diagnosis>Check ImageMagick installation, file formats, command syntax</diagnosis>
    <resolution>Reinstall dependencies, verify file formats, check parameters</resolution>
    <prevention>Use supported formats, test with small batches first</prevention>
  </media-processing-issues>
</troubleshooting>
```

### Debugging Strategies
```xml
<debugging-strategies>
  <systematic-approach>
    <isolate-problem>Test individual components separately</isolate-problem>
    <check-logs>Review console output and error messages</check-logs>
    <verify-dependencies>Ensure all required tools are installed</verify-dependencies>
    <test-incrementally>Make small changes and test frequently</test-incrementally>
  </systematic-approach>
  
  <common-debugging-tools>
    <browser-devtools>Inspect HTML, CSS, and JavaScript errors</browser-devtools>
    <terminal-output>Review script output for error messages</terminal-output>
    <file-verification>Check that files exist where expected</file-verification>
    <api-testing>Test API calls directly when needed</api-testing>
  </common-debugging-tools>
  
  <escalation-path>
    <documentation>Consult project documentation and README</documentation>
    <google-search>Search for similar issues and solutions</google-search>
    <community-resources>Check Neocities forums or developer communities</community-resources>
    <expert-consultation>Reach out to experienced developers when stuck</expert-consultation>
  </escalation-path>
</debugging-strategies>
```

## Workflow Optimization and Automation

### Time-Saving Practices
```xml
<optimization-practices>
  <shell-aliases>
    <setup>Create aliases for frequently used commands</setup>
    <examples>
      <deploy>alias deploy="npm run deploy"</deploy>
      <serve>alias serve="npm run serve"</serve>
      <check>alias check="npm run check-deps"</check>
    </examples>
  </shell-aliases>
  
  <editor-shortcuts>
    <markdown-snippets>Create snippets for common markdown patterns</markdown-snippets>
    <live-preview>Use editors with live markdown preview</live-preview>
    <auto-save>Configure auto-save to prevent work loss</auto-save>
  </editor-shortcuts>
  
  <batch-operations>
    <media-processing>Process multiple images in single command</media-processing>
    <content-creation>Write multiple posts before deploying</content-creation>
    <testing-cycles>Test multiple changes together when appropriate</testing-cycles>
  </batch-operations>
</optimization-practices>
```

### Future Automation Opportunities
```xml
<automation-opportunities>
  <content-workflow>
    <auto-deployment>Automatically deploy on Git push</auto-deployment>
    <content-scheduling>Schedule content publication</content-scheduling>
    <template-generation>Generate post templates with metadata</template-generation>
  </content-workflow>
  
  <media-workflow>
    <watch-folders>Automatically process new images/videos</watch-folders>
    <gallery-automation>Auto-update gallery.js from processed images</gallery-automation>
    <metadata-extraction>Extract image metadata automatically</metadata-extraction>
  </media-workflow>
  
  <quality-assurance>
    <automated-testing>Run tests before deployment</automated-testing>
    <link-checking>Automatically verify all links work</link-checking>
    <performance-monitoring>Monitor site performance metrics</performance-monitoring>
  </quality-assurance>
</automation-opportunities>
```

---

*This development workflow provides a structured approach to content creation and site management while maintaining flexibility for different types of work and creative processes.*
