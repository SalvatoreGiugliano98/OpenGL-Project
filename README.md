# Table of Contents
- [Installation](#installation)
    - [On Windows](#-on-windows)
    - [On macOS](#-on-macos)
    - [On Linux](#-on-linux)
- [Usage](#usage)
    - [Project Structure](#project-structure)
    - [Loading Resources](#loading-resources)
    - [Icon (Optional)](#icon-optional)
        - [On Windows](#on-windows)
        - [On macOS](#on-macos)
- [Build and Run](#build-and-run)
    - [On Windows](#-on-windows-1)
    - [On macOS](#-on-macos-)
    - [On Linux](#-on-linux-1)

# Installation
### 🪟 On Windows

When opening the project for the first time, select the `Visual Studio` toolchain and not `MinGW`.

Install vcpkg if you don't have it on your Windows system, pasting this line below in the terminal:
```bash
git clone https://github.com/microsoft/vcpkg.git
cd vcpkg
.\bootstrap-vcpkg.bat
```
Then install the dependencies (you should be in the vcpkg folder)
```bash
.\vcpkg.exe install freetype:x64-windows assimp:x64-windows glfw3:x64-windows openal-soft:x64-windows libsndfile:x64-windows
```
**ATTENTION**: It could take some minutes.

After the installation, in your **Project Folder**, run:
```bash
cmake -B build -DCMAKE_TOOLCHAIN_FILE=C:/Users/"YOUR USER NAME"/vcpkg/scripts/buildsystems/vcpkg.cmake
```
**IMPORTANT**: If the cmake command is not found, in the [Installation](Installation/cmake-4.2.0-rc1-windows-x86_64.msi) folder, there is the `.msi` file to install it.

### 🍎 On macOS 
You need Homebrew to install dependencies.
If you don't have it, install it by running this command in the terminal:
```bash
  /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
To check if everything is correct, run (after reopening the terminal):
```bash
  brew --version
```
If the installation was successful it should print something like: `Homebrew 4.x.x`

Use brew to install dependencies:
```bash
  brew install assimp glfw freetype libsndfile
```
To Change your App Name, go to the [CmakeLists.txt](CMakeLists.txt) at line 8 and change the name of the project.
And in the [Main](OpenGLApp/Main.cpp) at line 48.

### 🐧 On Linux
From the terminal, run:
```bash
  sudo apt update && sudo apt install -y \
    build-essential cmake pkg-config \
    libgl1-mesa-dev \
    libfreetype6-dev \
    libassimp-dev \
    libglfw3-dev \
    libopenal-dev \
    libsndfile1-dev

```

# Usage
## Project structure

The project is structured in the following way:
- [CMakeLists.txt](CMakeLists.txt)
- [glad](glad)
- [glm-master](glm-master)
- [Installation](Installation)
- [OpenGLApp](OpenGLApp)
  - [Main.cpp](OpenGLApp/Main.cpp)
  - [app_icon.rc](OpenGLApp/app_icon.rc)
  - [resources](OpenGLApp/resources)
  - [ViewController](OpenGLApp/ViewController)
  - [Game Classes](OpenGLApp/GameClasses)
  - [Data Classes](OpenGLApp/DataClasses)

The Main.cpp file is the entry point of the project. It creates the window, the OpenGL context, and some global variables.

From here, it loads the [Main View Controller](OpenGLApp/ViewController/MainViewController.cpp).
This is the first screen of the game. Right now there is a template background and a button to start the game (Not implemented yet).

In the [Game Classes](OpenGLApp/GameClasses) folder (_now empty_), you can add your own classes for the game, such as Player, GameManager, etc.

In the [Data Classes](OpenGLApp/DataClasses) folder, there are some classes used to load resources, such as the [shader.h](OpenGLApp/DataClasses/shader.h).
In addition, here, there are:
- [Button class](OpenGLApp/DataClasses/Button.h) 
- [Position class](OpenGLApp/DataClasses/Position.h) used to manage the position of the objects.
- [Sound Manager](OpenGLApp/DataClasses/SoundManager.h) and [Sound Engine](OpenGLApp/DataClasses/SoundEngine.h) used to load and play sounds.

**IMPORTANT**: You can change this structure, but do not move the _CMakeLists.txt_, _Main.cpp_, _app_icon.rc_, the _resources_' folder, the _glad_ and _glm-master_ folders. 
If you do, you will have to change the path in the CMakeLists.txt file.

**IMPORTANT**: If you want to add more directories, you have to add them in the [CMakeLists.txt](CMakeLists.txt) file at line 74.

## Loading Resources

The [resources](OpenGLApp/resources) directory must be in the same directory as the [Main.cpp](OpenGLApp/Main.cpp) file.
To load a resource, you have to use the function:
```c++
inline std::string getResource(const std::string& relativePath)
```
Defined in the [Main.cpp](OpenGLApp/Main.cpp) file at line 68.
Example:
```c++
menuMusic = SoundManager(getResource("Music/menu.mp3"),
                         soundEngine.volMusic,
                         true,
                         &soundEngine);
```

If you want to change this structure, you can change the [CmakeLists.txt](CMakeLists.txt) file.

### Icon (Optional)
#### On Windows
Convert the [Icon file](OpenGLApp/resources/Icon/Icon.png) to a file .ico.
You can use [this](https://convertio.co/es/png-ico/) website to do it.

Remember to save the .ico file in the [Icon](OpenGLApp/resources/Icon) directory with the same name `Icon.ico`. If not, you have to change the path in the [app_icon.rc](OpenGLApp/app_icon.rc)
 

#### On macOS
To use a costume icon, create a file named icon.png in the resources' directory.
Then in the terminal, from the project directory, run:
```bash
    mkdir -p OpenGLApp/resources/Icon/Icon.iconset
    cp OpenGLApp/resources/Icon/Icon.png OpenGLApp/resources/Icon/Icon.iconset/icon_512x512.png
    sips -z 16 16     OpenGLApp/resources/Icon/Icon.iconset/icon_512x512.png --out OpenGLApp/resources/Icon/Icon.iconset/icon_16x16.png
    sips -z 32 32     OpenGLApp/resources/Icon/Icon.iconset/icon_512x512.png --out OpenGLApp/resources/Icon/Icon.iconset/icon_16x16@2x.png
    sips -z 128 128   OpenGLApp/resources/Icon/Icon.iconset/icon_512x512.png --out OpenGLApp/resources/Icon/Icon.iconset/icon_128x128.png
    sips -z 256 256   OpenGLApp/resources/Icon/Icon.iconset/icon_512x512.png --out OpenGLApp/resources/Icon/Icon.iconset/icon_128x128@2x.png
    iconutil -c icns OpenGLApp/resources/Icon/Icon.iconset
```
If everything went well, you should have a file named `Icon.icns` in the [here](OpenGLApp/resources/Icon/Icon.icns).

# Build and Run

After installing all dependencies, you can build and run the project on your operating system of choice.

---

### 🪟 On Windows
1. Open a terminal in your project directory.
2. Run the following commands:
   ```bash
   cmake --build build
    ```
   Or use the build functions of your IDE.
3. Run the executable file in the [build](cmake-build-debug) folder.

### 🍎 On macOS
1. Open a terminal in your project directory.
2. Run the following commands:
   ```bash
   cmake -B build
   cmake --build build
    ```
   Or use the build functions of your IDE.
3. Run the .app bundle in the [build](cmake-build-debug) folder.

### 🐧 On Linux
1. Open a terminal in your project directory.
2. Run the following commands:
   ```bash
   cmake -B build
   cmake --build build
    ```
   Or use the build functions of your IDE.
3. Run the .app bundle in the [build](build) folder.
