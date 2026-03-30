# Ladybird Workspace Onboarding Guide

## Project Overview

**Ladybird** is a truly independent web browser using a novel engine based on web standards. This is a pre-alpha project written primarily in C++23 with some Rust components.

### Key Architecture
- **Multi-process architecture**: Main UI, WebContent renderers, ImageDecoder, RequestServer
- **Sandboxed tabs**: Each tab runs in its own renderer process
- **Cross-platform**: Linux, macOS, Windows (WSL2), iOS, Android

### Core Libraries (from SerenityOS)
- **LibWeb**: Web rendering engine
- **LibJS**: JavaScript engine  
- **LibWasm**: WebAssembly implementation
- **LibCrypto/LibTLS**: Cryptography & TLS
- **LibHTTP**: HTTP/1.1 client
- **LibGfx**: Graphics & image decoding
- **LibUnicode**: Unicode & locale support
- **LibMedia**: Audio/video playback
- **LibCore**: Event loop & OS abstraction
- **LibIPC**: Inter-process communication

---

## Workspace Structure

```
ladybird/
├── AK/                      # Utility container & algorithm library
├── Base/                    # Base system utilities
├── Documentation/           # Development & build documentation
├── Libraries/               # Core browser libraries (30+ libs)
│   ├── LibWeb/             # Main web rendering engine
│   ├── LibJS/              # JavaScript engine (with Rust components)
│   ├── LibWasm/            # WebAssembly support
│   ├── LibGfx/             # Graphics & image decoding
│   └── ... (LibCrypto, LibTLS, LibHTTP, etc.)
├── Services/                # Out-of-process services
│   ├── WebContent/         # Renderer process
│   ├── ImageDecoder/       # Image decoding service
│   ├── RequestServer/      # Network request service
│   └── WebDriver/          # WebDriver implementation
├── UI/                       # User interface
│   ├── Qt/                 # Qt-based UI (supported)
│   ├── AppKit/             # macOS AppKit UI (alternative)
│   └── Android/            # Android UI
├── Tests/                    # Test suite
├── Utilities/               # Utility programs
├── Meta/                    # Build configuration & scripts
│   ├── ladybird.py        # Main build script
│   ├── CMake/             # CMake configuration
│   └── find_compiler.py   # Compiler detection
├── Toolchain/               # Toolchain & build tools
├── CMakeLists.txt           # CMake project definition
├── Cargo.toml               # Rust workspace definition
├── CMakePresets.json        # Build presets (Debug/Release/Sanitizer)
└── vcpkg.json               # Dependency management

```

---

## System Status Check

### ✅ Already Installed on Your System

| Component | Status | Version | Notes |
|-----------|--------|---------|-------|
| CMake | ✅ | 4.3.1 | Supports modern build features |
| Ninja | ✅ | 1.13.2 | Fast build system |
| Clang | ✅ | 21.0.0 | Apple clang (C++23 capable) |
| Xcode CLI Tools | ✅ | Present | Installed via Xcode.app |
| Python 3 | ✅ | Available | Via pyenv |
| Build Tools | ✅ | Installed | libtool, nasm, autoconf, automake |
| pkg-config | ✅ | Present | Dependency discovery |
| OpenSSL | ✅ | 3.5.0 | Both openssl@1.1 and openssl@3 available |

### ❌ Missing Dependencies (Required for Build)

| Component | Status | Install Command |
|-----------|--------|-----------------|
| Qt6 | ❌ | `brew install qt` |
| Rust Toolchain | ❌ | `curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs \| sh` |
| ccache | ⚠️ Optional | `brew install ccache` (speeds up rebuilds) |

---

## Installation of Missing Dependencies

### Option 1: Install via Homebrew (Recommended for Qt)

```bash
# Install Qt6 user interface support
brew install qt

# Install Rust (if needed for development)
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env

# Install ccache for faster rebuilds
brew install ccache
```

### Option 2: Build Without Qt6

If you don't need the Qt UI, you can build with the AppKit UI instead:
```bash
./Meta/ladybird.py run
```

---

## Build System Architecture

### Build Tools Used
- **CMake 3.30+**: Project configuration
- **Ninja**: Fast parallel build system
- **vcpkg**: Package manager for C++ dependencies
- **Cargo**: Rust build system for LibJS, LibRegex, LibUnicode

### Build Presets Available
```bash
# Debug build (with symbols, slower)
BUILD_PRESET=Debug ./Meta/ladybird.py run

# Release build (optimized, faster) - DEFAULT
./Meta/ladybird.py run

# Sanitizer build (for debugging)
BUILD_PRESET=Sanitizer ./Meta/ladybird.py build
```

### Build Command Examples
```bash
# Simple usage - builds and runs Release build
./Meta/ladybird.py run

# Build only (no launch)
./Meta/ladybird.py build

# Run tests
./Meta/ladybird.py test

# Debug with lldb
./Meta/ladybird.py debug --debugger lldb ladybird

# Profile with Callgrind
./Meta/ladybird.py profile

# Run other applications (JS REPL, WebAssembly REPL)
./Meta/ladybird.py run js
./Meta/ladybird.py run wasm
```

---

## Quick Start Guide

### Step 1: Install Required Dependencies
```bash
# Install Qt and other tools
brew install qt ccache

# Install Rust (required for Rust components in LibJS)
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env
```

### Step 2: Verify Setup
```bash
cd /Users/ajpri/Documents/GitHub/ladybird

# Check if build script is executable
ls -la Meta/ladybird.py

# You should see: -rwxr-xr-x (executable)
```

### Step 3: First Build
```bash
cd /Users/ajpri/Documents/GitHub/ladybird

# Configure and build in Release mode (takes 10-30 min depending on system)
./Meta/ladybird.py build

# Once built, run the browser
./Meta/ladybird.py run
```

### Step 4: Debug Build (Optional)
```bash
# For development with debug symbols
BUILD_PRESET=Debug ./Meta/ladybird.py run
```

---

## Development Workflows

### Contributing to Ladybird

1. **Find an Issue**: Check [GitHub Issues](https://github.com/LadybirdBrowser/ladybird/issues) or look for TODOs in code
2. **Read Contributing Guidelines**: [CONTRIBUTING.md](./CONTRIBUTING.md)
3. **Set up Development Environment**: Follow this onboarding guide
4. **Create a Branch**: For your feature/fix
5. **Make Changes**: Follow Ladybird coding style
6. **Build & Test**: Use the build script
7. **Submit PR**: To the `master` branch

### Code Style & Tools

- **C++ Style**: See [CodingStyle.md](./Documentation/CodingStyle.md)
- **Format Code**: Uses `clang-format` (configured via `.clang-format`)
- **Linting**: `clang-tidy` configuration in `.clang-tidy`
- **Editor Config**: `.editorconfig` for consistency
- **Language**: American English, proper grammar, no contractions

### Testing

```bash
# Run all tests
./Meta/ladybird.py test

# Run specific test pattern
./Meta/ladybird.py test "regex_pattern"

# Import Web Platform Tests (WPT)
# See: Documentation/Testing.md
```

---

## Important Documentation Files

| File | Purpose |
|------|---------|
| [BuildInstructionsLadybird.md](./Documentation/BuildInstructionsLadybird.md) | Complete build setup guide |
| [AdvancedBuildInstructions.md](./Documentation/AdvancedBuildInstructions.md) | Advanced configurations |
| [GettingStartedContributing.md](./Documentation/GettingStartedContributing.md) | Contribution workflow |
| [ProcessArchitecture.md](./Documentation/ProcessArchitecture.md) | System architecture overview |
| [LibWebFromLoadingToPainting.md](./Documentation/LibWebFromLoadingToPainting.md) | Web rendering pipeline |
| [Testing.md](./Documentation/Testing.md) | Test infrastructure & WPT |
| [CodingStyle.md](./Documentation/CodingStyle.md) | Code style guidelines |
| [Troubleshooting.md](./Documentation/Troubleshooting.md) | Common build issues |
| [FAQ.md](./Documentation/FAQ.md) | Frequently asked questions |

---

## IDE/Editor Configuration

### VS Code
- See: [Documentation/EditorConfiguration/VSCodeConfiguration.md](./Documentation/EditorConfiguration/VSCodeConfiguration.md)
- Use Clangd extension for C++ IntelliSense
- `.clangd` configuration file is provided

### CLion
- See: [Documentation/EditorConfiguration/CLionConfiguration.md](./Documentation/EditorConfiguration/CLionConfiguration.md)

### Other Editors
- Vim, NeoVim, Emacs, Helix, Qt Creator all have configuration guides

---

## Custom Build Scripts

### Main Build Script: `./Meta/ladybird.py`

This Python script handles:
- Compiler detection and selection
- CMake configuration
- Building with ninja
- Running the browser
- Debugging with lldb/gdb
- Profiling with Callgrind
- Installation

Example environment variables:
```bash
# Use specific compilers
CC=clang CXX=clang++ ./Meta/ladybird.py run

# Set build preset
BUILD_PRESET=Debug ./Meta/ladybird.py run

# Parallel jobs
./Meta/ladybird.py build --jobs 8
```

---

## Troubleshooting

### Build Fails on First Try
1. Check [Troubleshooting.md](./Documentation/Troubleshooting.md)
2. Ensure all dependencies are installed: `brew install qt`
3. Verify Rust is installed: `rustc --version`
4. Check CMAKE at least 3.30: `cmake --version`
5. Ask in Discord: [Ladybird Discord](https://discord.gg/nvfjVJ4Svh)

### Common Issues on macOS
- **Qt not found**: Install via `brew install qt`
- **Slow first build**: Normal, can take 30+ minutes
- **Rust compiler errors**: Ensure Rust is up to date: `rustup update`
- **linker errors**: Check OpenSSL: `brew list | grep openssl`

### Getting Help
- **Discord**: [Official Discord Server](https://discord.gg/nvfjVJ4Svh) - #build-problems channel
- **Documentation**: [Troubleshooting.md](./Documentation/Troubleshooting.md)
- **GitHub Issues**: [Issues Tracker](https://github.com/LadybirdBrowser/ladybird/issues)

---

## Project Statistics

- **Language**: Primarily C++23, with Rust components
- **Libraries**: 30+ core libraries
- **Service Processes**: 4 main out-of-process services
- **Architecture**: Multi-process, sandboxed design
- **CI/CD**: GitHub Actions with Linux, macOS, Windows builds

---

## Next Steps

1. **✅ Install Missing Packages**: 
   ```bash
   brew install qt ccache
   curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
   ```

2. **🔨 First Build**:
   ```bash
   cd /Users/ajpri/Documents/GitHub/ladybird
   ./Meta/ladybird.py build
   ```

3. **🚀 Run the Browser**:
   ```bash
   ./Meta/ladybird.py run
   ```

4. **📚 Read Documentation**:
   - [ProcessArchitecture.md](./Documentation/ProcessArchitecture.md) - Understanding the system
   - [GettingStartedContributing.md](./Documentation/GettingStartedContributing.md) - If planning to contribute

5. **🐞 Find Issues**:
   - Look for `good first issue` labels
   - Check [WPT failures](https://wpt.fyi/results/?label=master&product=ladybird)
   - Search for TODO/FIXME comments in code

---

## Revision History

- **Created**: 2025-03-30
- **Status**: Active Ladybird Development Environment
- **macOS Deployment Target**: 14.0+
- **CMake**: 4.3.1+
- **Clang**: 21.0.0+ (C++23 capable)

