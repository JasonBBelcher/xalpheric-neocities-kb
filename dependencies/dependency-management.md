# Dependency Management System

## Overview

The Xalpheric project implements a **comprehensive dependency management system** that automatically detects, validates, and helps install required dependencies across all processing scripts. The system provides interactive installation prompts and platform-specific guidance to ensure smooth setup and operation.

## Dependency Architecture

### Multi-Layer Dependency Structure
```xml
<dependency-layers>
  <system-dependencies>
    <imagemagick>
      <purpose>Image processing and optimization</purpose>
      <command>magick</command>
      <scripts>process-photos.js</scripts>
      <installation>
        <macos>brew install imagemagick</macos>
        <ubuntu>sudo apt install imagemagick</ubuntu>
        <windows>Download from imagemagick.org</windows>
      </installation>
    </imagemagick>
    
    <ffmpeg>
      <purpose>Video to audio conversion</purpose>
      <command>ffmpeg</command>
      <scripts>process-video.js</scripts>
      <installation>
        <macos>brew install ffmpeg</macos>
        <ubuntu>sudo apt install ffmpeg</ubuntu>
        <windows>Download from ffmpeg.org</windows>
      </installation>
    </ffmpeg>
    
    <jq>
      <purpose>JSON processing for video workflows</purpose>
      <command>jq</command>
      <scripts>process-video.js</scripts>
      <installation>
        <macos>brew install jq</macos>
        <ubuntu>sudo apt install jq</ubuntu>
        <windows>Download from jqlang.github.io</windows>
      </installation>
    </jq>
  </system-dependencies>
  
  <nodejs-dependencies>
    <core-packages>
      <form-data>File uploads to Neocities API</form-data>
      <markdown-it>Markdown to HTML conversion</markdown-it>
      <dotenv>Environment variable management</dotenv>
      <chokidar>File watching capabilities</chokidar>
    </core-packages>
    <dev-dependencies>
      <http-server>Local development server</http-server>
    </dev-dependencies>
  </nodejs-dependencies>
  
  <platform-specific>
    <macos>
      <sqlite3>Usually pre-installed, for Photos app integration</sqlite3>
      <osascript>Built-in AppleScript support</osascript>
    </macos>
    <permissions>
      <photos-access>Required for watch-photos functionality</photos-access>
      <full-disk-access>May be required for some operations</full-disk-access>
    </permissions>
  </platform-specific>
</dependency-layers>
```

## Automatic Dependency Detection

### Detection Framework
```xml
<detection-system>
  <command-checking>
    <function>checkCommand(commandName)</function>
    <method>
      <attempt>Execute command with --version or --help</attempt>
      <success-criteria>Exit code 0 or expected output</success-criteria>
      <failure-handling>Catch exceptions and return false</failure-handling>
    </method>
    <implementation>
      try {
        execSync(`${command} --version`, { stdio: 'ignore' });
        return true;
      } catch {
        return false;
      }
    </implementation>
  </command-checking>
  
  <nodejs-package-checking>
    <function>checkNodePackage(packageName)</function>
    <method>
      <require-attempt>Try to require() the package</require-attempt>
      <success-criteria>No exception thrown</success-criteria>
      <failure-handling>Catch require errors</failure-handling>
    </method>
    <implementation>
      try {
        require(packageName);
        return true;
      } catch {
        return false;
      }
    </implementation>
  </nodejs-package-checking>
  
  <validation-timing>
    <script-startup>Check dependencies before main execution</script-startup>
    <early-exit>Stop execution if critical dependencies missing</early-exit>
    <user-interaction>Prompt for installation assistance</user-interaction>
  </validation-timing>
</detection-system>
```

### Detection Coverage by Script

```xml
<script-dependencies>
  <process-photos>
    <system-deps>
      <imagemagick>Critical - stops execution if missing</imagemagick>
    </system-deps>
    <fallback>Platform-specific installation instructions</fallback>
  </process-photos>
  
  <process-video>
    <system-deps>
      <ffmpeg>Critical - stops execution if missing</ffmpeg>
      <jq>Critical - stops execution if missing</jq>
    </system-deps>
    <validation>Both must be present for video processing</validation>
  </process-video>
  
  <deployment-scripts>
    <nodejs-deps>
      <form-data>File upload functionality</form-data>
      <markdown-it>Content processing</markdown-it>
      <dotenv>Configuration management</dotenv>
    </nodejs-deps>
    <auto-install>Offers to install missing Node packages</auto-install>
  </deployment-scripts>
  
  <watch-photos>
    <platform-specific>
      <macos-only>Requires macOS for Photos app integration</macos-only>
      <photos-app>Photos.app must be installed and configured</photos-app>
      <permissions>System permissions for database access</permissions>
    </platform-specific>
  </watch-photos>
</script-dependencies>
```

## Interactive Installation System

### Installation Prompts
```xml
<installation-prompts>
  <user-interaction>
    <prompt-format>
      "🤔 Would you like me to install the missing {type} packages? (y/N): "
    </prompt-format>
    <default-behavior>No (safe default)</default-behavior>
    <timeout>No timeout - waits for user input</timeout>
  </user-interaction>
  
  <nodejs-packages>
    <detection>Scan for missing required packages</detection>
    <batch-installation>Install all missing packages in single npm command</batch-installation>
    <command>npm install {package1} {package2} {package3}</command>
    <success-verification>Re-check package availability after installation</success-verification>
  </nodejs-packages>
  
  <system-packages>
    <platform-detection>Identify macOS, Linux, Windows</platform-detection>
    <instruction-display>Show platform-specific installation commands</instruction-display>
    <manual-process>User must install system packages manually</manual-process>
    <verification>Re-run script after installation to verify</verification>
  </system-packages>
</installation-prompts>
```

### Installation Workflow
```xml
<installation-workflow>
  <nodejs-installation>
    <process>
      <step>Detect missing Node.js packages</step>
      <step>Display list of missing packages</step>
      <step>Prompt user for installation permission</step>
      <step>If approved: Execute npm install command</step>
      <step>Verify installation success</step>
      <step>Continue with script execution</step>
    </process>
    <error-handling>
      <npm-failure>Display error and manual installation instructions</npm-failure>
      <permission-denied>Suggest sudo or different npm configuration</permission-denied>
      <network-issues>Suggest checking internet connection</network-issues>
    </error-handling>
  </nodejs-installation>
  
  <system-installation>
    <process>
      <step>Detect missing system commands</step>
      <step>Display platform-specific installation instructions</step>
      <step>Wait for user to install manually</step>
      <step>Suggest re-running script to verify</step>
    </process>
    <platform-guidance>
      <macos>Homebrew commands provided</macos>
      <ubuntu>apt package manager commands</ubuntu>
      <centos>yum package manager commands</centos>
      <windows>Download links and manual instructions</windows>
    </platform-guidance>
  </system-installation>
</installation-workflow>
```

## Platform-Specific Handling

### macOS Optimizations
```xml
<macos-support>
  <homebrew-integration>
    <detection>Check if Homebrew is available</detection>
    <recommendations>Suggest Homebrew for missing packages</recommendations>
    <commands>
      <imagemagick>brew install imagemagick</imagemagick>
      <ffmpeg>brew install ffmpeg</ffmpeg>
      <jq>brew install jq</jq>
    </commands>
  </homebrew-integration>
  
  <system-integration>
    <photos-app>
      <requirement>Photos.app must be installed</requirement>
      <database-access>Requires permission to access Photos database</database-access>
      <applescript>Uses osascript for system integration</applescript>
    </photos-app>
    <permissions>
      <full-disk-access>May be required for some operations</full-disk-access>
      <photos-access>Required for watch-photos functionality</photos-access>
    </permissions>
  </system-integration>
  
  <file-handling>
    <heic-support>Native HEIC support through ImageMagick</heic-support>
    <airdrop-workflow>Optimized for iPhone photo workflows</airdrop-workflow>
  </file-handling>
</macos-support>
```

### Linux Support
```xml
<linux-support>
  <package-managers>
    <ubuntu-debian>
      <imagemagick>sudo apt install imagemagick</imagemagick>
      <ffmpeg>sudo apt install ffmpeg</ffmpeg>
      <jq>sudo apt install jq</jq>
    </ubuntu-debian>
    <rhel-centos>
      <imagemagick>sudo yum install ImageMagick</imagemagick>
      <ffmpeg>sudo yum install ffmpeg</ffmpeg>
      <jq>sudo yum install jq</jq>
    </rhel-centos>
  </package-managers>
  
  <considerations>
    <repository-enablement>May need to enable additional repositories</repository-enablement>
    <compilation>Some packages may need compilation from source</compilation>
    <permissions>User must have sudo access for installation</permissions>
  </considerations>
</linux-support>
```

### Windows Support
```xml
<windows-support>
  <installation-challenges>
    <no-package-manager>No standardized package manager</no-package-manager>
    <manual-downloads>Most tools require manual download and installation</manual-downloads>
    <path-configuration>Tools must be added to system PATH</path-configuration>
  </installation-challenges>
  
  <provided-guidance>
    <imagemagick>
      <url>https://imagemagick.org/script/download.php</url>
      <notes>Download Windows installer, ensure PATH is configured</notes>
    </imagemagick>
    <ffmpeg>
      <url>https://ffmpeg.org/download.html</url>
      <notes>Download static build, extract to PATH directory</notes>
    </ffmpeg>
    <jq>
      <url>https://jqlang.github.io/jq/download/</url>
      <notes>Download Windows executable, place in PATH</notes>
    </jq>
  </provided-guidance>
  
  <alternative-solutions>
    <wsl>Windows Subsystem for Linux for Unix-style environment</wsl>
    <chocolatey>Third-party package manager option</chocolatey>
    <scoop>Alternative package manager for developers</scoop>
  </alternative-solutions>
</windows-support>
```

## Error Handling and User Guidance

### Error Categorization
```xml
<error-handling>
  <dependency-missing>
    <detection>Command not found or package not available</detection>
    <response>Clear error message with installation instructions</response>
    <examples>
      <imagemagick-missing>
        "❌ ImageMagick not found. Install with: brew install imagemagick"
      </imagemagick-missing>
    </examples>
  </dependency-missing>
  
  <installation-failure>
    <npm-errors>Network issues, permission problems, or package conflicts</npm-errors>
    <system-errors>Package manager issues or missing repositories</system-errors>
    <response>Fallback to manual installation instructions</response>
  </installation-failure>
  
  <permission-issues>
    <sudo-required>System package installation needs elevated privileges</sudo-required>
    <file-access>Script cannot read/write required files</file-access>
    <photos-access>macOS permissions for Photos app access</photos-access>
  </permission-issues>
  
  <version-compatibility>
    <outdated-versions>Installed tools may be too old</outdated-versions>
    <incompatible-versions>Tools may have breaking changes</incompatible-versions>
    <detection>Version checking could be enhanced</detection>
  </version-compatibility>
</error-handling>
```

### User Guidance System
```xml
<user-guidance>
  <progressive-disclosure>
    <basic-errors>Simple, actionable error messages</basic-errors>
    <detailed-help>Additional information available on request</detailed-help>
    <troubleshooting>Common solutions for frequent issues</troubleshooting>
  </progressive-disclosure>
  
  <platform-awareness>
    <detection>Automatically detect user's operating system</detection>
    <tailored-instructions>Show relevant installation commands only</tailored-instructions>
    <fallback-options>Multiple installation methods when available</fallback-options>
  </platform-awareness>
  
  <recovery-assistance>
    <retry-prompts>Suggest re-running script after manual installation</retry-prompts>
    <verification-help>Guide users through dependency verification</verification-help>
    <common-fixes>Address frequent installation issues</common-fixes>
  </recovery-assistance>
</user-guidance>
```

## Dependency Validation Framework

### Validation Functions
```xml
<validation-framework>
  <checkDependencies-function>
    <purpose>Main orchestration function for each script</purpose>
    <process>
      <step>Check all required dependencies</step>
      <step>Collect missing dependencies</step>
      <step>Display status for each dependency</step>
      <step>Offer installation assistance if needed</step>
      <step>Exit gracefully if critical dependencies missing</step>
    </process>
  </checkDependencies-function>
  
  <attemptInstall-function>
    <purpose>Try to install missing system dependencies</purpose>
    <platform-detection>Identify OS and available package managers</platform-detection>
    <command-execution>Execute appropriate installation commands</command-execution>
    <success-verification>Verify installation worked</success-verification>
  </attemptInstall-function>
  
  <promptInstall-function>
    <purpose>Interactive user permission for installations</purpose>
    <readline-integration>Use readline for user input</readline-integration>
    <clear-prompts>Explain what will be installed</clear-prompts>
    <safe-defaults>Default to 'no' to prevent unwanted installations</safe-defaults>
  </promptInstall-function>
</validation-framework>
```

## Performance and Caching

### Dependency Check Optimization
```xml
<performance-optimization>
  <caching-opportunities>
    <dependency-status>Cache results between script runs</dependency-status>
    <version-information>Cache version checks to avoid repeated calls</version-information>
    <installation-status>Remember successful installations</installation-status>
  </caching-opportunities>
  
  <current-limitations>
    <repeated-checks>Every script run checks all dependencies</repeated-checks>
    <no-persistence>No memory of previous checks</no-persistence>
    <redundant-validation>Same tools checked by multiple scripts</redundant-validation>
  </current-limitations>
  
  <improvement-opportunities>
    <shared-cache>Central dependency status cache</shared-cache>
    <timestamp-checking>Only re-check after certain time periods</timestamp-checking>
    <selective-validation>Only check dependencies needed for specific operations</selective-validation>
  </improvement-opportunities>
</performance-optimization>
```

## Integration with Build System

### Script Integration Patterns
```xml
<build-integration>
  <startup-sequence>
    <step>Load dependency checking functions</step>
    <step>Define required dependencies for this script</step>
    <step>Execute checkDependencies() before main logic</step>
    <step>Handle missing dependencies with user interaction</step>
    <step>Proceed with main script execution if all dependencies satisfied</step>
  </startup-sequence>
  
  <consistency-patterns>
    <shared-functions>Common dependency checking code reused across scripts</shared-functions>
    <standard-messages>Consistent error messages and user guidance</standard-messages>
    <uniform-behavior>Same user experience across all scripts</uniform-behavior>
  </consistency-patterns>
  
  <development-workflow>
    <new-script-creation>Copy dependency checking patterns from existing scripts</new-script-creation>
    <dependency-addition>Add new dependencies to appropriate checking functions</dependency-addition>
    <testing>Verify dependency checking works in clean environments</testing>
  </development-workflow>
</build-integration>
```

## Future Enhancement Opportunities

### Advanced Dependency Management
```xml
<enhancement-opportunities>
  <centralized-management>
    <dependency-manifest>Central file listing all project dependencies</dependency-manifest>
    <version-requirements>Specify minimum required versions</version-requirements>
    <optional-dependencies>Mark some dependencies as optional with fallbacks</optional-dependencies>
  </centralized-management>
  
  <automatic-installation>
    <package-manager-detection>Automatically detect available package managers</package-manager-detection>
    <unattended-installation>Option for non-interactive installation</unattended-installation>
    <docker-integration>Container-based dependency management</docker-integration>
  </automatic-installation>
  
  <health-monitoring>
    <periodic-checks>Regular validation of dependency health</periodic-checks>
    <update-notifications>Alert when dependencies have updates available</update-notifications>
    <compatibility-checking>Verify dependencies work together</compatibility-checking>
  </health-monitoring>
  
  <user-experience>
    <gui-installer>Graphical interface for dependency installation</gui-installer>
    <progress-indicators>Show installation progress</progress-indicators>
    <rollback-capability>Undo dependency installations if needed</rollback-capability>
  </user-experience>
</enhancement-opportunities>
```

---

*The dependency management system ensures reliable operation across different environments while providing helpful guidance for setup and troubleshooting.*
