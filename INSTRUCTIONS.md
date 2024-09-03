## Here is a step-by-step guide to set up Visual Studio Code for compiling and debugging C++20 code using g++-13 on WSL.
Setting Up Visual Studio Code for C++20 Development
## Prerequisites
1.	Visual Studio Code: Ensure you have Visual Studio Code installed.
2.	WSL: Ensure you have Windows Subsystem for Linux (WSL) installed and set up.
3. g++-13: Ensure g++-13 is installed on your WSL instance. You can install it using the following commands:
```bash
sudo apt update
sudo apt install g++-13

```
## Step-by-Step Guide
### 1. Create `tasks.json`
Create a tasks.json file in the .vscode directory of your workspace with the following content:
```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "build",
            "type": "shell",
            "command": "/usr/bin/g++-13",
            "args": [
                "-std=c++20",
                "-g",
                "${file}",
                "-o",
                "${fileDirname}/${fileBasenameNoExtension}"
            ],
            "group": {
                "kind": "build",
                "isDefault": true
            },
            "problemMatcher": [
                "$gcc"
            ],
            "detail": "Generated task by user"
        }
    ]
}

```
### 2. Create `launch.json`
Create a launch.json file in the .vscode directory of your workspace with the following content:
```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "C/C++: g++-13 build and debug active file",
            "type": "cppdbg",
            "request": "launch",
            "program": "${fileDirname}/${fileBasenameNoExtension}",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": false,
            "MIMode": "gdb",
            "miDebuggerPath": "/usr/bin/gdb",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "preLaunchTask": "build",
            "internalConsoleOptions": "openOnSessionStart"
        }
    ]
}

```
### 3. Verify Your Source Code
Ensure your C++ source code is correctly checking the __cplusplus macro. Here is an example `main.cpp`:

```cpp
#include <iostream>

int main() {
    std::cout << "Hello, World!" << std::endl;
    std::cout << "__cplusplus: " << __cplusplus << std::endl;
    return 0;
}

```
### 4. Build and Debug

1. Build the Project: Open the terminal in Visual Studio Code and run the build task by pressing `Ctrl+Shift+B`.

2. Run the Executable: Start debugging by pressing `F5`.

### 5. Expected Output
When you run the application, you should see the following output in the console:

```bash
Hello, World!
__cplusplus: 202002L
```