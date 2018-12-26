---
title: "Boost & Visual Studio Code"
excerpt_separator: "<!--more-->"
excerpt: "Setting up Visual Studio Code for C++ Boost library."
description: "Setting up Visual Studio Code for Boost library"
categories:
  - Tutorials
header:
    og_image: /assets/images/simplicity3.png
tags:
  - C++
  - Boost
  - Visual Studio Code
  - CMake
---
{% include toc title="Contents" icon="list-ul" %}

## Introduction

Microsoft's Visual Studio product ever since the '90s (was  Visual C++ back then) has been my primary C++ IDE and I still love using it for the majority of my coding needs. Unfortunately it hasn't been as convenient to use since after I switched to Macbook. I'm not a fan of boot camp, so I have been using virtualization software to run Visual Studio and the load on the system has depricated the experience.

A while ago Microsoft's cross platform editor Visual Studio Code caught my eye so I gave it a go. Judging by my experience so far, VS Code will probably change my habit, at least for solo non-critical projects. It supports a variety of scripting languages out of the box, but how was the experience for native C++ development with Boost ? Try for yourselves.

## Steps

Unlike Visual Studio, Visual Studio Code doesn't support C++ language out of the box. Luckily, it has a great built-in marketplace. I will be using CMake as the building environment. This way, your code should compile and run on every platform without any need of modification whatosever. CMake is beautiful.

There is an official guide for C++ [here](https://code.visualstudio.com/docs/languages/cpp) which is not CMake oriented. I also had several problems in successfully running the code as described here.

### Prerequisites

Download and install Visual Studio Code from official [download site](https://code.visualstudio.com/)
{: .notice}

Download and install CMake from official [download site](https://cmake.org/download/)
{: .notice}

You also need standard C++ libraries. Installing XCode on Mac or Visual Studio on Windows should take care of that. Otherwise you need to install them manually and define include directories in `c_cpp_properties.json`

### C++ Extensions

* Install [C/C++ extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools). This is an official Microsoft extension.
* Install [CMake extension](https://marketplace.visualstudio.com/items?itemName=twxs.cmake). 
* Install [CMake Tools extension](https://marketplace.visualstudio.com/items?itemName=vector-of-bool.cmake-tools). This enables usage of a set of CMake commands from inside VS Code.
* I also installed [Native Debugger](https://marketplace.visualstudio.com/items?itemName=webfreak.debug).

Here is the official guide on extensions.
{% include video id="Fed01v3yYNE" provider="youtube" %}

### Creating Project
Visual Studio Code works with folders. Create a folder at your projects directory. Let's say "HelloBoost". Open the folder from VS Code's File menu.

* Create new file (`⌘N` on Mac) and name it main.cpp.

```cpp
#include <iostream>
using namespace std;

int main(int argc, char* argv[])
{
  cout << "Hello World!" << endl;
  return 0;
}
```

* Create another file and name it "CMakeLists.txt"

```cmake
cmake_minimum_required(VERSION 3.8)
set (CMAKE_CXX_STANDARD 14)
project(HelloBoost)

set (VERSION_MAJOR 1)
set (VERSION_MINOR 0)

set(SOURCE main.cpp)

add_executable(${PROJECT_NAME} ${SOURCE})
```

### Json Files
Visual Studio Code configurations work with JSon files inside `./vscode` subfolder of the project. In the end, we are going to have three JSon files in there. 

* First one is `cmaketools.json` Not going into details of CMake itself, but this is how it works on Visual Studio Code.

Open Command Palette, `⇧⌘P` on Mac, or from View menu. Run `>CMake: Build` command. Select `Debug`.

![CMake]({{ site.url }}{{ site.baseurl }}/assets/images/cmakebuild.png){:height="50px" width="500px"}

This should create the following structure:

![CMake]({{ site.url }}{{ site.baseurl }}/assets/images/cmakeafterbuild.png){:height="240px" width="250px"}

* Second is `c_cpp_properties.json`, which determines the include directories. To create this file open Command Palette and run `>C/Cpp: Edit Configurations`. You may then edit this file to change include directories for each configuration for various operating systems. On Mac, we are assuming XCode is installed. This file looks like this;

```json
{
    "configurations": [
        {
            "name": "Mac",
            "includePath": [
                "/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/include/c++/v1",
                "/usr/local/include",
                "/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/lib/clang/8.1.0/include",
                "/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/include",
                "/usr/include",
                "${workspaceRoot}"
            ],
            "defines": [],
            "intelliSenseMode": "clang-x64",
            "browse": {
                "path": [
                    "/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/include/c++/v1",
                    "/usr/local/include",
                    "/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/lib/clang/8.1.0/include",
                    "/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/include",
                    "/usr/include",
                    "${workspaceRoot}"
                ],
                "limitSymbolsToIncludedHeaders": true,
                "databaseFilename": ""
            },
            "macFrameworkPath": [
                "/System/Library/Frameworks",
                "/Library/Frameworks"
            ]
        },
        {
            "name": "Linux",
            "includePath": [
                "/usr/include",
                "/usr/local/include",
                "${workspaceRoot}"
            ],
            "defines": [],
            "intelliSenseMode": "clang-x64",
            "browse": {
                "path": [
                    "/usr/include",
                    "/usr/local/include",
                    "${workspaceRoot}"
                ],
                "limitSymbolsToIncludedHeaders": true,
                "databaseFilename": ""
            }
        },
        {
            "name": "Win32",
            "includePath": [
                "C:/Program Files (x86)/Microsoft Visual Studio 14.0/VC/include",
                "${workspaceRoot}"
            ],
            "defines": [
                "_DEBUG",
                "UNICODE"
            ],
            "intelliSenseMode": "msvc-x64",
            "browse": {
                "path": [
                    "C:/Program Files (x86)/Microsoft Visual Studio 14.0/VC/include/*",
                    "${workspaceRoot}"
                ],
                "limitSymbolsToIncludedHeaders": true,
                "databaseFilename": ""
            }
        }
    ],
    "version": 3
}
```
* `launch.json` is the final one which tells the debugger what to do. Open Command Pallete and run `>Debug: Open launch.json`. The dropdown will ask you to select environment. Select `C++ (GDB/LLDB)`. This should create launch.json file which should look something like below;

```json
{
        "version": "0.2.0",
        "configurations": [
            {
                "name": "(lldb) Launch",
                "type": "cppdbg",
                "request": "launch",
                "program": "enter program name, for example ${workspaceRoot}/a.out",
                "args": [],
                "stopAtEntry": false,
                "cwd": "${workspaceRoot}",
                "environment": [],
                "externalConsole": true,
                "MIMode": "lldb"
            }
        ]
    }
```

All you need to do is change `"program"` line to 

`"program": "${workspaceRoot}/build/HelloBoost",`

I also change `externalConsole` to `false` since I prefer using VS Code's built-in debugger console.

This is the final look on files.
![CMake]({{ site.url }}{{ site.baseurl }}/assets/images/cmakefinal.png){:height="240px" width="250px"}

### C++ Debugging
Now everything you need to debug C++ code is set-up. From the Command Palette run `>CMake: Build` again. Once it is built, put a breakpoint (if you like) into your main function and hit `F5`. Debugger should hit your breakpoint, and you can move onto next line with `F10` as you normally would on Visual Studio. VS Code's built-in Debug Console should display "Hello World".

### Installing Boost

Download the appropriate Boost library from [here](http://www.boost.org/users/download/).
{: .notice}

You can use the prebuilt windows binaries or build it yourself for Mac. Here is how to do it on mac; 

Download the .tar.gz, extract it, open a terminal (or use VS Code's built-in terminal) and build it using Clang.

On terminal;

```sh
./bootstrap.sh --prefix=/usr/local/boost-1.65.1
sudo ./b2 cxxflags=-std=c++14 install
```

### CMake with Boost

Edit CMakeLists.txt file and change it to;

```cmake
cmake_minimum_required(VERSION 3.8)
set (CMAKE_CXX_STANDARD 14)
project(HelloBoost)

set (VERSION_MAJOR 1)
set (VERSION_MINOR 0)

set(BOOST_ROOT "/usr/local/boost-1.65.1")
set(Boost_USE_STATIC_LIBS        ON)  # only find static libs
set(Boost_USE_DEBUG_LIBS         OFF) # ignore debug libs and
set(Boost_USE_RELEASE_LIBS       ON)  # only find release libs
set(Boost_USE_MULTITHREADED      ON)
set(Boost_USE_STATIC_RUNTIME    OFF)

set(SOURCE main.cpp)

find_package(Boost 1.65.1 COMPONENTS thread regex system)
if(Boost_FOUND)
  include_directories(${Boost_INCLUDE_DIRS})
  add_executable(${PROJECT_NAME} ${SOURCE})
  target_link_libraries(${PROJECT_NAME} ${Boost_THREAD_LIBRARY} ${Boost_REGEX_LIBRARY} ${Boost_SYSTEM_LIBRARY})
endif()
```

Normally find_package should be able to find boost, but I included BOOST_ROOT directory in case it fails for you. Also set options to use static-multithread-release libraries. Most of the boost libraries are header only, but I added several non-header boost libraries just to show how it is done.
You may also want to edit `c_cpp_properties.json` file and add boost include path (`"/usr/local/boost-1.65.1/include",` for me). 

To make sure boost libraries are ready, change `main.cpp` file to;

```cpp
#include <boost/math/special_functions/sign.hpp>
#include <iostream>
using namespace std;

int main(int argc, char* argv[])
{
  cout << copysign(4.2, 7.9) << endl;
  return 0;
}
```

Finally from the command palette, run `>CMake: Clean`,  `>CMake:Build` and hit `F5` to debug.

## The End

If all is well and running, you can enjoy using Visual Studio Code as a decent IDE for some high performance development action with C++ & Boost. So far, I'm having a decent experience under Visual Studio Code and hope you feel the same.

Adios,

Ayhan