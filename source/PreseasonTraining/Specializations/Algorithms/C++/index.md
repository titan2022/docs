# C++ lessons

[Slideshow](https://docs.google.com/presentation/d/1188XNZOsVpBr-WWFrqIOh8J3RmE63SAxxBUVuUOgLo8/edit#slide=id.g31071c5d512_0_13)

(6 hours)

## Prerequisites (Debian)

1. Boot into Debian and open a terminal
2. Type in the following commands (one by one) and install everything
```bash
sudo apt update

sudo apt install git
sudo apt install build-essential
sudo apt install cmake
sudo apt install clang-17
sudo apt install lldb
```
- `sudo apt update` will update the repository list on your computer
- `git` is git
- `build-essential` is a bundle of necessary packages for c++ like `make` and `gcc`, just to make sure you have them
- `cmake` is our build system, will be covered in lesson
- `clang-17` is our compiler, will be covered in lesson
- `lldb` is debugger for `clang`, will be covered in lesson

3. Install VSCode on Debian if you do not have it, [follow this guide](<https://code.visualstudio.com/docs/setup/linux>)
4. Install [Clangd extension](<https://marketplace.visualstudio.com/items?itemName=llvm-vs-code-extensions.vscode-clangd>) in VSCode
5. Install [Clang-format extension](<https://marketplace.visualstudio.com/items?itemName=xaver.clang-format>) in VSCode
6. Install [CMake tools extension](<https://marketplace.visualstudio.com/items?itemName=ms-vscode.cmake-tools>) in VSCode
