---
Title: UIP 1 - Introduction
tags: uip c++ game 2.5d sdl2 project platformer
toc: false
sidebar:
  - title: "Project repository"
    text: "<https://github.com/dojitza/uip>"
#hidden
published: false
---

Hello and welcome to the introduction of my new project.

I will be making a 2.5d platformer game, its purpose being learning advanced c++ and surrounding tools.  
I intend the game to be multi-platform and to support several low level graphic libraries for rendering.
Because of this I will try to abstract the rendering library away which will hopefully be a good lesson in modularity.  
I am not very familiar with graphic libraries, so I am going to use [SDL](https://www.libsdl.org/) – a simple 2d library built around openGL.

This project will include building my own rendering engine and writing compatibility layers around concrete libraries (only SDL at first). I intend to build game logic in parallel, adding features to the renderer as required by the game. Also, I will be writing a series of posts documenting as much of the development process as possible. This is intended as an insight into my process, but might also come in handy for someone tackling a similar project.

That's it! Stay tuned for the next post, where I will talk about the dreaded process of setting up a development environment.




