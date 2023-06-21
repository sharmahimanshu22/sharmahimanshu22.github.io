---
title: Realearnig OpenGL
layout: post
---

What's a context  ?

There are two kinds of context relevant here:

1. A rendering context - We will also call this OpenGL context
2. A device context - Also called "window" or a "drawable"


In any OpenGL function call, a context is implicit which is set at the beginning. If this is not set, we can expect <b>nothing</b> to happen.

GLFW a library - provides an object <b>GLFWwindow</b> which serves both as a rendering context and a device context.


```cpp
// creating context object
GLFWwindow* window = glfwCreateWindow(640, 480, "My Title", NULL, NULL);
```

```cpp
// setting the object to be current context
glfwMakeContextCurrent(window);
```


<b> -- Work In Progress --</b>