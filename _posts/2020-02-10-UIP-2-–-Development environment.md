---
Title: UIP 2 - Development environment
tags: uip game msvc clang cmake make sdl2 windows linux
sidebar:
  - title: "Project repository"
    text: "<https://github.com/dojitza/uip>"
  - title: "Source tag"
    text: "[blogpost2](https://github.com/dojitza/uip/releases/tag/blogpost2)"
---

Hello! In this post I will talk about setting up the development environment for my UIP project.

I want this project to be as flexible as possible in terms of technology. Of course, the language is fixed to be c++ but the rest of the toolchain should be interchangeable.  
For this reason, I've decided to use [CMake](cmake.org) as my build system.  
CMake isn't a build system per say, but a generator of build files which are then used by a concrete build system itself. It can generate build files for several build systems including make, ninja and even visual studio. In this project, I used cmake will be generating makefiles.

The compiler I use on windows is MSVC, and on linux I opted for CLang. I've heard/read good things about CLang and would like to give it a shot.

I am not sure how this is going to turn out, but I hope that the hurdles I encounter will highlight the differences in these compilers and how those affect the development process.

## Cmake and SDL
This is the project structure that I will be starting with.

```
├── CMakeLists.txt
├── LICENSE
├── README.md
├── cmake
│   └── FindSDL2.cmake
└── src
    └── main.cpp
```

As I mentioned in my [introductory post]({% post_url 2020-02-09-UIP-1-–-Introduction %}), I will be starting out with SDL2 as my graphics library. In order to use SDL2, we need to make sure our project can link against it and include it's headers.
Note the `FindSDL2.cmake` file. This is required on Windows to find the SDL2 library. We will provide this file to cmake and it will find the library for us. This isn't all of it though.  
A very helpful [blog post](https://trenki2.github.io/blog/2017/06/02/using-sdl2-with-cmake/) pointed out that there is an additional file needed for cmake to sucessfuly find SDL2 on windows. It should be put directly into the folder of the unpacked SDL2 development library.

The file should be named `sdl2-config.cmake` and here are it's contents:


```cmake
set(SDL2_INCLUDE_DIRS "${CMAKE_CURRENT_LIST_DIR}/include")

# Support both 32 and 64 bit builds
if (${CMAKE_SIZEOF_VOID_P} MATCHES 8)
  set(SDL2_LIBRARIES "${CMAKE_CURRENT_LIST_DIR}/lib/x64/SDL2.lib;${CMAKE_CURRENT_LIST_DIR}/lib/x64/SDL2main.lib")
else ()
  set(SDL2_LIBRARIES "${CMAKE_CURRENT_LIST_DIR}/lib/x86/SDL2.lib;${CMAKE_CURRENT_LIST_DIR}/lib/x86/SDL2main.lib")
endif ()

string(STRIP "${SDL2_LIBRARIES}" SDL2_LIBRARIES)
```

And here are the contents of CMakelists.txt


```cmake
cmake_minimum_required(VERSION 3.10.2)
project(uip)

IF(UNIX)
set(CMAKE_C_COMPILER "clang")
set(CMAKE_CXX_COMPILER "clang++")
ENDIF()

set(CMAKE_CXX_STANDARD 17)

set(CMAKE_PREFIX_PATH
        ${CMAKE_PREFIX_PATH}
        "C:\\DevLibraries\\SDL2-2.0.10"
        )

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/")

add_executable(uip src/main.cpp)

find_package(SDL2 REQUIRED)

include_directories(${SDL2_INCLUDE_DIR})
target_link_libraries(uip ${SDL2_LIBRARY})
```
Along the usual cmake drill, notice the lines setting `CMAKE_PREFIX_PATH`, `CMAKE_MODULE_PATH`, and the `find_package` command. `CMAKE_PREFIX_PATH` is a variable which is used by the `find_xxx` commands (`find_package` in this case). It is supposed to contain the paths to root directories of the files that are to be found. `CMAKE_MODULE_PATH`, on the other hand points to the directories containing modules that search for the packages (in this case, `projectroot/cmake` which contains `FindSDL2.cmake`).

You may notice 2 set commands defining the compiler surrounded by a conditional. This is to set the compiler only if we are on unix. The compiler is otherwise selected by cmake based on the environment variables and arguments to the program. This is handled by the IDE on Windows, but for my Linux tests I had to manually set the compiler for now since I was not using an IDE.

## SDL2 test program

To test our build configuration I will copy a simple program from a popular [SDL2 tutorial by Lazy Foo](http://lazyfoo.net/tutorials/SDL/01_hello_SDL/index2.php).

```cpp
#include <SDL.h>
#include <stdio.h>

//Screen dimension constants
const int SCREEN_WIDTH = 640;
const int SCREEN_HEIGHT = 480;

int main(int argc, char* args[]) {
    //The window we'll be rendering to
    SDL_Window* window = NULL;
    //The surface contained by the window
    SDL_Surface* screenSurface = NULL;
    //Initialize SDL
    if( SDL_Init( SDL_INIT_VIDEO ) < 0 )
    {
        printf( "SDL could not initialize! SDL_Error: %s\n", SDL_GetError());
    }
    else
    {
        //Create window
        window = SDL_CreateWindow( "SDL Tutorial", SDL_WINDOWPOS_UNDEFINED, SDL_WINDOWPOS_UNDEFINED,
            SCREEN_WIDTH, SCREEN_HEIGHT, SDL_WINDOW_SHOWN );
        if( window == NULL )
        {
            printf( "Window could not be created! SDL_Error: %s\n", SDL_GetError() );
        }
        else
        {
            //Get window surface
            screenSurface = SDL_GetWindowSurface( window );
            //Fill the surface white
            SDL_FillRect( screenSurface, NULL, SDL_MapRGB( screenSurface->format, 0xFF, 0xFF, 0xFF ) );
            //Update the surface
            SDL_UpdateWindowSurface( window );
            //Wait two seconds
            SDL_Delay( 2000 );
        }
    }
    return 0;
}
```

This program should create a window, fill it with white color, and close it after 2 seconds. 

![Program Window](/assets/images/first_window.png)

Success! The test program builds and runs!

That marks the end of this post. Stay tuned for more and see you soon!




