# VS Code Setup for Ladybird Development

## Recommended VS Code Extensions

### Essential
- **Clangd** (`llvm-vs-code-extensions.vscode-clangd`) - C++ IntelliSense and diagnostics
- **CMake Tools** (`ms-vscode.cmake-tools`) - CMake support and build integration  
- **Git Graph** (`mhutchie.git-graph`) - Git visualization

### Optional but Helpful
- **CodeLLDB** (`vadimcn.vscode-lldb`) - LLDB debugging
- **Rust Analyzer** (`rust-lang.rust-analyzer`) - Rust support for Rust components
- **Better Comments** (`aaron-bond.better-comments`) - Highlight TODO/FIXME comments

## VS Code Settings for Ladybird

### settings.json

Add these to your workspace settings:

```json
{
    // C++ and Build Settings
    "C_Cpp.codeAnalysis.runAutomatically": false,
    "C_Cpp.default.intelliSenseEngine": "disabled",
    "[cpp]": {
        "editor.defaultFormatter": "xaver.clang-format",
        "editor.formatOnSave": true,
        "editor.rulers": [80, 120]
    },
    
    // Clangd Configuration
    "clangd.fallbackFlags": [
        "-std=c++23",
        "-fPIC"
    ],
    "clangd.serverCompletionRanking": true,
    
    // CMake Configuration
    "cmake.configureEnvironment": {
        "CC": "clang",
        "CXX": "clang++"
    },
    "cmake.generator": "Ninja",
    "cmake.buildDirectory": "${workspaceFolder}/build",
    "cmake.sourceDirectory": "${workspaceFolder}",
    
    // Editor General Settings
    "editor.rulers": [80, 120],
    "editor.trimAutoWhitespace": true,
    "files.trimTrailingWhitespace": true,
    "files.insertFinalNewline": true,
    "[markdown]": {
        "editor.wordWrap": "on"
    },
    
    // Search Ignore Patterns
    "search.exclude": {
        "**/node_modules": true,
        "**/build": true,
        "**/.git": true,
        "**/vcpkg_installed": true
    }
}
```

### launch.json (for Debugging)

Add this debug configuration:

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Ladybird (LLDB)",
            "type": "lldb",
            "request": "launch",
            "program": "${workspaceFolder}/build/bin/ladybird",
            "args": [],
            "cwd": "${workspaceFolder}",
            "stopOnEntry": false,
            "environment": [],
            "externalConsole": false,
            "preLaunchTask": "Ladybird: Build Release"
        },
        {
            "name": "Ladybird (Debug Build)",
            "type": "lldb",
            "request": "launch",
            "program": "${workspaceFolder}/build-debug/bin/ladybird",
            "args": [],
            "cwd": "${workspaceFolder}",
            "stopOnEntry": false,
            "environment": [],
            "externalConsole": false,
            "preLaunchTask": "Ladybird: Build Debug"
        }
    ]
}
```

### tasks.json (for Build Tasks)

Add these build tasks:

```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "Ladybird: Build Release",
            "type": "shell",
            "command": "${workspaceFolder}/Meta/ladybird.py",
            "args": ["build"],
            "group": {
                "kind": "build",
                "isDefault": true
            },
            "presentation": {
                "reveal": "always",
                "panel": "shared"
            },
            "problemMatcher": {
                "pattern": {
                    "regexp": "^(.*):(\\d+):(\\d+): (error|warning): (.*)$",
                    "file": 1,
                    "line": 2,
                    "column": 3,
                    "severity": 4,
                    "message": 5
                }
            }
        },
        {
            "label": "Ladybird: Build Debug",
            "type": "shell",
            "command": "bash",
            "args": ["-c", "cd ${workspaceFolder} && BUILD_PRESET=Debug ./Meta/ladybird.py build"],
            "group": "build",
            "presentation": {
                "reveal": "always",
                "panel": "shared"
            }
        },
        {
            "label": "Ladybird: Run",
            "type": "shell",
            "command": "${workspaceFolder}/Meta/ladybird.py",
            "args": ["run"],
            "group": "build",
            "presentation": {
                "reveal": "always",
                "panel": "shared"
            }
        },
        {
            "label": "Ladybird: Tests",
            "type": "shell",
            "command": "${workspaceFolder}/Meta/ladybird.py",
            "args": ["test"],
            "group": "test",
            "presentation": {
                "reveal": "always",
                "panel": "shared"
            }
        }
    ]
}
```

## Keyboard Shortcuts

Add to `keybindings.json`:

```json
[
    {
        "key": "shift+cmd+b",
        "command": "workbench.action.tasks.runTask",
        "args": "Ladybird: Build Release"
    },
    {
        "key": "shift+cmd+r",
        "command": "workbench.action.tasks.runTask",
        "args": "Ladybird: Run"
    },
    {
        "key": "shift+cmd+t",
        "command": "workbench.action.tasks.runTask",
        "args": "Ladybird: Tests"
    }
]
```

## Using VS Code Tasks

After adding the task definitions, you can:

1. **Build**: `Cmd+Shift+B` → Select "Ladybird: Build Release"
2. **Run tests**: `Cmd+Shift+P` → Select "Ladybird: Tests"
3. **Debug**: Press `F5` to start debugging with LLDB

## Useful Commands

In VS Code command palette (`Cmd+Shift+P`):

- `CMake: Select Kit` - Choose compiler
- `CMake: Configure` - Run CMake configuration
- `CMake: Build` - Build with CMake
- `Clangd: Show AST` - Inspect AST at cursor
- `Clangd: Check AST` - Check code semantics

## Search Tips

- Search for TODO/FIXME: `@tag/TODO` or `@tag/FIXME` in search
- Search across Libraries: `path:Libraries/LibWeb` in search
- Find symbols: `@symbol MyClass` in search

## Recommended Reading for VS Code

- [VS Code - Working with C++](https://code.visualstudio.com/docs/languages/cpp)
- [CMake Tools Extension](https://github.com/microsoft/vscode-cmake-tools)
- [Clangd Extension](https://clangd.llvm.org/installation.html)

