# Apache Thrift 0.20.0 with Qt6 Support for macOS

This repository contains Apache Thrift 0.20.0 with patches to support Qt6 on macOS and fixes for JavaScript build issues.

## Changes from Original

1. **Qt6 Support**: Added Qt6 support alongside Qt5
2. **JavaScript Build Fixes**: Updated QUnit to 2.19.4 to fix test compatibility issues
3. **Build System Updates**: Various CMake improvements for better macOS compatibility

## Prerequisites

Before building, ensure you have the following installed:

### Required Dependencies

```bash
# Install build tools
brew install cmake bison flex

# Install core dependencies
brew install boost libevent openssl@3 zlib glib

# For JavaScript support
brew install node ant

# Optional: Qt6 (if you want Qt support)
brew install qt@6
```

### Important Notes

- Bison 3.x is required (macOS default 2.3 is too old)
- Use dependencies from either Homebrew OR MacPorts, not both
- Java/Gradle support is disabled due to compatibility issues with Gradle 8.x

## Building from Source

### Quick Build (Recommended)

```bash
# Clone the repository
git clone https://github.com/automatedrules/thrift_20_qt6_macos.git
cd thrift_20_qt6_macos

# Create build directory
mkdir build
cd build

# Configure (without Qt6)
cmake .. \
  -DCMAKE_BUILD_TYPE=Release \
  -DCMAKE_INSTALL_PREFIX=/usr/local \
  -DWITH_JAVA=OFF \
  -DWITH_QT6=OFF \
  -DWITH_JAVASCRIPT=ON \
  -DWITH_NODEJS=ON

# Build
make -j$(sysctl -n hw.ncpu)

# Test (optional)
make check

# Install
sudo make install
```

### Build with Qt6 Support

If you have Qt6 installed and want to build with Qt6 support:

```bash
cmake .. \
  -DCMAKE_BUILD_TYPE=Release \
  -DCMAKE_INSTALL_PREFIX=/usr/local \
  -DWITH_JAVA=OFF \
  -DWITH_QT6=ON \
  -DWITH_JAVASCRIPT=ON \
  -DWITH_NODEJS=ON \
  -DCMAKE_PREFIX_PATH="/opt/homebrew/opt/qt@6;/opt/homebrew"
```

**Note**: Qt6 support on macOS requires additional configuration due to framework structure. You may need to create symlinks or adjust paths.

### Build Options

- `-DWITH_QT6=ON/OFF` - Enable/disable Qt6 support (default: OFF)
- `-DWITH_JAVASCRIPT=ON/OFF` - Enable/disable JavaScript support (default: ON)
- `-DWITH_NODEJS=ON/OFF` - Enable/disable Node.js support (default: ON)
- `-DWITH_JAVA=OFF` - Java support is disabled due to Gradle compatibility issues
- `-DCMAKE_PREFIX_PATH` - Add paths to find dependencies

## What Gets Installed

- `/usr/local/bin/thrift` - The Thrift compiler
- `/usr/local/lib/libthrift.a` - Core C++ library
- `/usr/local/lib/libthriftnb.a` - Non-blocking server library (with libevent)
- `/usr/local/lib/libthriftz.a` - Zlib transport
- `/usr/local/include/thrift/` - C++ headers
- `/usr/local/lib/cmake/thrift/` - CMake configuration files

Language-specific libraries:
- C++ static libraries
- C (GLib) libraries
- Python library (installed via setup.py)
- Node.js library
- JavaScript library

## Supported Languages

This build supports the following languages:
- C++ (with libevent for non-blocking servers)
- C (GLib)
- Python
- JavaScript
- Node.js

## Known Issues

1. **Qt6 on macOS**: Qt6 framework structure may require additional configuration
2. **Java/Gradle**: Disabled due to Gradle 8.x compatibility issues on macOS
3. **Some warnings during build**: These are mostly deprecation warnings and don't affect functionality

## Troubleshooting

### Bison Version Error
If you get "Bison version 3.0 or newer is required", ensure `/usr/local/bin` is in your PATH before `/usr/bin`:
```bash
export PATH="/usr/local/bin:$PATH"
```

### Qt6 Include Path Issues
If building with Qt6 fails with include path errors, you may need to create symlinks or use the non-Qt6 build.

### JavaScript Test Failures
The JavaScript tests have been fixed by updating to QUnit 2.19.4. If you still encounter issues, ensure you have the latest Node.js installed.

## License

Apache Thrift is licensed under the Apache License 2.0. See the LICENSE file for details.

## Original Repository

This is based on Apache Thrift 0.20.0. For the original source, see:
https://github.com/apache/thrift