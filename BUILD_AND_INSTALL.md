# Building and Installing Prism Launcher

This guide explains how to build Prism Launcher from source and create a distributable application package.

## Prerequisites

Before building, make sure you have the necessary dependencies installed. For detailed build instructions, see the [official build instructions](https://prismlauncher.org/wiki/development/build-instructions).

**Minimum Requirements:**
- CMake 3.22 or newer
- Qt 6.x
- C++20 compatible compiler (GCC, Clang, or MSVC)
- Java Runtime Environment (for running Minecraft)

## Building the Application

### 1. Configure the Build

Create a build directory and configure CMake:

```bash
mkdir build
cd build
cmake ..
```

Or use a specific build type:

```bash
cmake -DCMAKE_BUILD_TYPE=Release ..
```

### 2. Compile the Source Code

Build the project:

```bash
cmake --build . --config Release
```

This will compile all the necessary binaries and resources.

## Installing and Packaging

After building, you need to install the application to create a distributable package. The installation process varies by platform.

### Linux

#### Standard Installation

Install to the default system directories (requires root privileges):

```bash
sudo cmake --install . --config Release
```

This installs:
- Binary: `/usr/local/bin/prismlauncher`
- Resources: `/usr/local/share/prismlauncher/`
- Desktop files: `/usr/local/share/applications/`
- Icons: `/usr/local/share/icons/hicolor/`
- Man page: `/usr/local/share/man/man6/`

#### Custom Installation Directory

Install to a custom directory without root privileges:

```bash
cmake --install . --prefix /path/to/install/directory
```

#### Portable Installation

Create a portable installation that can be moved to any directory:

```bash
cmake --install . --prefix ./install --component portable
```

This creates a self-contained installation with:
- The launcher binary
- A launcher script
- All necessary resources

To run the portable version:

```bash
cd install
./prismlauncher
```

### Windows

#### Standard Installation

```bash
cmake --install . --config Release
```

This creates a directory with:
- `PrismLauncher.exe` - Main application
- `prismlauncher_filelink.exe` - File linking utility
- `prismlauncher_updater.exe` - Update utility
- All Qt libraries and dependencies
- Resources and assets

The installation directory can be moved to any location and the application will run.

#### Custom Installation Directory

```bash
cmake --install . --prefix "C:\PrismLauncher" --config Release
```

#### Runtime Component Only

To install only the main application without development files:

```bash
cmake --install . --component Runtime --config Release
```

### macOS

#### Application Bundle Installation

```bash
cmake --install . --config Release
```

This creates a complete `.app` bundle:
- `PrismLauncher.app/Contents/MacOS/PrismLauncher` - Executable
- `PrismLauncher.app/Contents/Resources/` - Resources and assets
- `PrismLauncher.app/Contents/Frameworks/` - Qt frameworks and Sparkle updater

The `.app` bundle can be moved to `/Applications` or any other location.

#### Custom Installation Directory

```bash
cmake --install . --prefix ~/Applications --config Release
```

## Installation Components

The CMake install system provides several components:

1. **Runtime** - Main application executable and required resources
2. **bundle** - Qt dependencies and libraries (automatically included)
3. **portable** - Additional files for portable installations (Linux only)

### Installing Specific Components

To install only specific components:

```bash
# Install only the runtime component
cmake --install . --component Runtime

# Install the bundle component (Qt libraries)
cmake --install . --component bundle

# Install portable files (Linux)
cmake --install . --component portable
```

## Platform-Specific Notes

### Linux

- The build system uses KDE Install Directories for standard paths
- Desktop integration files (`.desktop`, mime types) are automatically installed
- Use the `Launcher_BUILD_PLATFORM` CMake variable to set a distribution identifier:
  ```bash
  cmake -DLauncher_BUILD_PLATFORM=archlinux ..
  ```

### Windows

- Qt deployment is handled automatically via `qt_generate_deploy_script`
- Debug symbols (PDB files) are installed separately and are optional
- The application uses static linking where possible to reduce dependencies

### macOS

- Creates a complete application bundle with embedded frameworks
- Includes Sparkle framework for automatic updates
- Uses `macdeployqt` internally for dependency resolution
- The bundle is code-signed if you have configured signing certificates

## Creating Distributable Packages

### Linux

For Linux distributions, you typically create packages using your distribution's packaging tools:

- **Debian/Ubuntu**: Create a `.deb` package using `dpkg-deb` or `debuild`
- **Fedora/RHEL**: Create an `.rpm` package using `rpmbuild`
- **Arch Linux**: Create a PKGBUILD and use `makepkg`
- **AppImage**: Use `linuxdeploy` with the installed files
- **Flatpak**: See the `flatpak/` directory for Flatpak manifests

### Windows

1. Install the application to a directory
2. Create a ZIP archive of the installation directory, or
3. Use an installer creator like NSIS or Inno Setup

### macOS

1. Install the application to create the `.app` bundle
2. Create a DMG disk image:
   ```bash
   hdiutil create -volname "Prism Launcher" -srcfolder PrismLauncher.app -ov -format UDZO PrismLauncher.dmg
   ```

## Build Configuration Options

### Important CMake Variables

- `CMAKE_BUILD_TYPE` - Build type: Debug, Release, RelWithDebInfo, MinSizeRel
- `CMAKE_INSTALL_PREFIX` - Installation directory prefix
- `Launcher_BUILD_PLATFORM` - Platform identifier for your build
- `BUILD_TESTING` - Enable/disable tests (default: ON)
- `Launcher_QT_VERSION_MAJOR` - Qt version to use (5 or 6)

### Example: Custom Configuration

```bash
cmake -DCMAKE_BUILD_TYPE=Release \
      -DCMAKE_INSTALL_PREFIX=/opt/prismlauncher \
      -DLauncher_BUILD_PLATFORM=custom \
      ..
```

## Troubleshooting

### Missing Dependencies

If you get errors about missing libraries:

1. **Linux**: Ensure Qt6 development packages are installed
2. **Windows**: Make sure Qt is in your PATH or use `-DCMAKE_PREFIX_PATH`
3. **macOS**: Install Qt via Homebrew or use the official Qt installer

### Installation Errors

If `cmake --install` fails:

1. Check you have write permissions to the installation directory
2. Use `--prefix` to specify a different location
3. Use `sudo` for system-wide installations on Linux/macOS

### Runtime Errors

If the installed application doesn't run:

1. **Linux**: Check that Qt libraries are in your library path
2. **Windows**: Ensure all DLLs are in the same directory as the executable
3. **macOS**: Verify the `.app` bundle is complete and not corrupted

## Additional Resources

- [Official Build Instructions](https://prismlauncher.org/wiki/development/build-instructions)
- [Contributing Guidelines](CONTRIBUTING.md)
- [Project Website](https://prismlauncher.org)
- [GitHub Repository](https://github.com/PrismLauncher/PrismLauncher)

## License

Prism Launcher is licensed under the GPL-3.0-only license. See [LICENSE](LICENSE) for details.
