# Downloading Built Applications from GitHub Actions

This guide explains how to download pre-built application packages after GitHub Actions completes a build.

## Where to Find Built Applications

### 1. From Pull Requests

If you're looking for a build from a pull request:

1. Navigate to the pull request on GitHub
2. Scroll down to the **Checks** section at the bottom of the conversation
3. Click on **Details** next to the "Build" check
4. On the workflow run page, scroll down to the **Artifacts** section at the bottom
5. Download the artifact(s) you need

### 2. From the Actions Tab

You can also find builds directly from the Actions tab:

1. Go to the repository on GitHub: `https://github.com/PrismLauncher/PrismLauncher`
2. Click on the **Actions** tab at the top
3. Click on **Build** in the left sidebar (under "Workflows")
4. Click on any workflow run to see its details
5. Scroll down to the **Artifacts** section at the bottom
6. Download the artifact(s) you need

### 3. From nightly.link

For the latest development build from the `develop` branch, you can use nightly.link:

**Direct Link:** https://nightly.link/PrismLauncher/PrismLauncher/workflows/build/develop

This provides direct download links for the latest successful build without needing to navigate through GitHub's UI.

## Available Artifacts

After a successful build, the following artifacts are typically available:

### Linux

- **PrismLauncher-Linux-Qt6-Portable-[version]-[build-type]**
  - Portable tarball (`.tar.gz`) that works on most Linux distributions
  - Extract and run `./prismlauncher` from the extracted directory

- **PrismLauncher-Linux-[version]-[build-type]-[arch].AppImage**
  - AppImage format for easy distribution
  - Make it executable: `chmod +x PrismLauncher-*.AppImage`
  - Run directly: `./PrismLauncher-*.AppImage`

- **PrismLauncher-Linux-[version]-[build-type]-[arch].AppImage.zsync**
  - Update file for AppImage (used by AppImageUpdate)

### Windows

- **PrismLauncher-Windows-[variant]-[version]-[build-type]**
  - Standard Windows build (folder with all files)
  - Extract the ZIP and run `prismlauncher.exe`

- **PrismLauncher-Windows-[variant]-Portable-[version]-[build-type]**
  - Portable Windows build
  - Extract the ZIP and run `prismlauncher.exe`
  - Includes a portable launcher script for easier usage

- **PrismLauncher-Windows-[variant]-Setup-[version]-[build-type]**
  - Windows installer (`.exe`)
  - Run the installer to install Prism Launcher system-wide

Windows builds are available in multiple variants:
- **MinGW (x86_64)**: Built with MinGW compiler (x86_64)
- **MinGW (ARM64)**: Built with MinGW compiler (ARM64)
- **MSVC (x86_64)**: Built with Microsoft Visual C++ compiler (x86_64)
- **MSVC (ARM64)**: Built with Microsoft Visual C++ compiler (ARM64)

### macOS

- **PrismLauncher-macOS-[version]-[build-type]**
  - Contains `PrismLauncher.zip` with the `.app` bundle
  - Extract the ZIP and move `Prism Launcher.app` to your Applications folder
  - Universal binary supporting both Intel and Apple Silicon

## Understanding Artifact Names

Artifacts follow this naming pattern:
```
PrismLauncher-[Platform]-[Variant]-[Version]-[BuildType]
```

- **Platform**: `Linux`, `Windows`, `macOS`
- **Variant**: Specific build variant (e.g., `MSVC`, `MinGW-w64`)
- **Version**: Git commit hash (e.g., `abc1234`)
- **BuildType**: `Debug` or `Release`

### Build Types

- **Debug**: Includes debug symbols and additional logging
  - Larger file size
  - Useful for troubleshooting issues
  - May run slower

- **Release**: Optimized for performance
  - Smaller file size
  - Recommended for general use
  - Better performance

## Tips

1. **Download the right architecture**:
   - For most modern PCs: use x86_64/amd64 builds
   - For newer ARM-based Windows PCs: use arm64 builds
   - For Apple Silicon Macs: the universal binary works on both Intel and Apple Silicon

2. **Artifact expiration**:
   - GitHub automatically deletes artifacts after 90 days
   - Released versions are available permanently from the Releases page

3. **File sizes**:
   - Debug builds are significantly larger than Release builds due to debug symbols
   - AppImages and portable builds include all dependencies

4. **Linux distributions**:
   - AppImages work on most distributions without installation
   - Portable tarballs also work universally
   - For permanent installation, use your distribution's package manager if available

## Downloading Without GitHub Account

GitHub requires you to be logged in to download artifacts from Actions. If you don't have a GitHub account:

1. Use **nightly.link** (mentioned above) for the latest develop builds
2. Wait for official releases on the [Releases page](https://github.com/PrismLauncher/PrismLauncher/releases)
3. Use distribution-specific packages (AUR, Flathub, etc.) listed on the [website](https://prismlauncher.org/download)

## Troubleshooting

### "Artifacts not found"

If you don't see artifacts:
- The workflow may still be running (check the status)
- The build may have failed (check the workflow logs)
- Artifacts may have expired (only kept for 90 days)

### "Build is for pull requests from forks"

Some security features may be limited for fork PRs:
- Windows builds may not be code-signed
- macOS builds may not be notarized
- You can still download and use them, but may see security warnings

### File won't run or open

- **Linux AppImage**: Make sure it's executable: `chmod +x PrismLauncher-*.AppImage`
- **Windows**: Your antivirus may flag unsigned builds from PRs as suspicious
- **macOS**: For ad-hoc signed builds, you may need to allow the app in System Preferences → Security & Privacy

## Additional Resources

- [Official Downloads](https://prismlauncher.org/download) - Stable releases and distribution packages
- [Build Instructions](./BUILD_AND_INSTALL.md) - For building from source locally
- [GitHub Actions](https://github.com/PrismLauncher/PrismLauncher/actions) - View all workflow runs
- [nightly.link](https://nightly.link/PrismLauncher/PrismLauncher/workflows/build/develop) - Latest develop builds
