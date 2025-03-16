## GLFW Setup

### Window setup

Always include OpenGL functions library before all

```cpp
#include <GL/glew.h>
#include <GLFW/glfw3.h>
```

Then, proceed to setup the window.

```cpp
if(!glfwInit()) {
	return -1;
}
glfwWindowHint(GLFW_RESIZABLE, GLFW_TRUE);
glfwWindowHint(GLFW_DECORATED, GLFW_TRUE);
GLFWwindow* window = glfwCreateWindow(WDW_WIDTH, WDW_HEIGHT, "SDF", NULL, NULL);

if(window == NULL) {
	glfwTerminate();
	return -1;
}

glfwMakeContextCurrent(window);
glewExperimental = GL_TRUE;
GLenum err = glewInit();

if(GLEW_OK != err) {
	std::cout<<"GLEW failed to initialize"<<std::endl;
	return -1;
}

glViewport(0, 0, WDW_WIDTH, WDW_HEIGHT); // Rendering place
```

### Rendering loop

```cpp
while(!glfwWindowShouldClose(window)) {
	glClearColor(0.2f, 0.3f, 0.3f, 1.0f);
    glClear(GL_COLOR_BUFFER_BIT);
    glfwSwapBuffers(window);
    glfwPollEvents();
}
glfwDestroyWindow(window);
glfwTerminate();
```

## Creating data and sending to shader

### First triangle

We define a first triangle using the normalize device coordinates (small space where the x, y and z values vary from -1.0 to 1.0, any coordinates outside this range will be discared / clipped and won't be visible on screen).

```cpp
float vertices[] {
	-0.5f, -0.5f, 0.0f,
	0.5f, -0.5f, 0.0f,
	0.0f, 0.5f, 0.0f
}:
```

 ### Vertex Buffer Object (VBO)

We then create a VBO, that is used to store the vertices in the GPU memory, allowing us to send the data all at once on the GPU.

```cpp
unsigned int VBO;
glGenBuffers(1, &VBO);
```

We bind it on the GPU on the GL_ARRAY_BUFFER target.

```cpp
glBindBuffer(GL_ARRAY_BUFFER, VBO);
```

And we pass the data to the bound GL_ARRAY_BUFFER buffer using

```cpp
glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);
```

Key differences :

- `GL_STREAM_DRAW` : the data is set only once and used by the GPU at most a few times
- `GL_STATIC_DRAW` : the data is set only once and used many times
- `GL_DYNAMIC_DRAW` : the data is changed a lot and used many times

Depending on the type, the GPU will allow slower or faster memory placement for the data.

Then, to send to data the shader and tell him how to interpret it, we use glVertexAttribPointer :

```c++
glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void*)0);
glEnableVertexAttribArray(0);
```

- The first parameter specifies which vertex attribute we want to configure. Here, we have `layout (location = 0)` in the vertex shader. This sets the location of the vertex attribute to `0` and since we want to pass data to this vertex attribute, we pass in `0`.
- The next argument specifies the size of the vertex attribute. The vertex attribute is a `vec3` so it is composed of `3` values.
- The third argument specifies the type of the data which is GL_FLOAT (a `vec*` in GLSL consists of floating point values).
- The next argument specifies if we want the data to be normalized. If we're inputting integer data types (int, byte) and we've set this to GL_TRUE, the integer data is normalized to `0` (or `-1` for signed data) and `1` when converted to float. This is not relevant for us so we'll leave this at GL_FALSE.
- The fifth argument is known as the stride and tells us the space between consecutive vertex attributes. Since the next set of position data is located exactly 3 times the size of a `float` away we specify that value as the stride. Note that since we know that the array is tightly packed (there is no space between the next vertex attribute value) we could've also specified the stride as `0` to let OpenGL determine the stride (this only works when values are tightly packed). Whenever we have more vertex attributes we have to carefully define the spacing between each vertex attribute.
- The last parameter is of type `void*` and thus requires that weird cast. This is the offset of where the position data begins in the buffer. Since the position data is at the start of the data array this value is just `0`.
### Sending the data using VAO

VAO (Vertex Array Object) stores the configuration of the vertex attribute and the associated VBO. It stores the state of a given VBO.

A vertex array object stores the following:

- Calls to `glEnableVertexAttribArray` or `glDisableVertexAttribArray`
- Vertex attribute configurations via `glVertexAttribPointer`
- Vertex buffer objects associated with vertex attributes by calls to `glVertexAttribPointer`

To use it, we simply generate it :

```c++
unsigned int VAO;
glGenVertexArrays(1, &VAO);
```

And then we bind it before doing anything with the VBO :

```c++
glBindVertexArray(VAO);
```

Here is what it looks like in the end :

```c++
unsigned int VBO;
unsigned int VAO;
glGenBuffers(1, &VBO);
glGenVertexArrays(1, &VAO);
glBindVertexArray(VAO);
glBindBuffer(GL_ARRAY_BUFFER, VBO);
glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);
glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void*)0);
glEnableVertexAttribArray(0);
glBindVertexArray(0);
```
### Creating shaders

Vertex shader is responsible for processing the vertices sent by the application. Most basic vertex shader

```glsl
#version 330 core
layout(location = 0) in vec3 aPos;

void main() {
    gl_Position = vec4(aPos.x, aPos.y, aPos.z, 1.0);
}
```

Fragment shader is responsible for calculating the color output of each pixel. Most basic fragment shader :

```glsl
#version 330 core
out vec4 FragColor;

void main() {
    FragColor = vec4(1.0f, 0.5f, 0.2f, 1.0f); // Orange color
}
```

### Compiling and linking shaders

Using a Shader class to make it easier, here is the constructor :

```c++
Shader::Shader(const char* vertex_path, const char* fragment_path) {
    // Reading shaders
    std::string vertex_code;
    std::string fragment_code;
    std::ifstream vertex_shader_file;
    std::ifstream fragment_shader_file;
    vertex_shader_file.exceptions(std::ifstream::failbit | std::ifstream::badbit);
    fragment_shader_file.exceptions(std::ifstream::failbit | std::ifstream::badbit);
    try {
        vertex_shader_file.open(vertex_path);
        fragment_shader_file.open(fragment_path);
        std::stringstream vertex_shader_stream;
        std::stringstream fragment_shader_stream;
        vertex_shader_stream << vertex_shader_file.rdbuf();
        fragment_shader_stream << fragment_shader_file.rdbuf();
        vertex_shader_file.close();
        fragment_shader_file.close();
        vertex_code = vertex_shader_stream.str();
        fragment_code = fragment_shader_stream.str();
    }
    catch(std::ifstream::failure e) {
        std::cout<<"ERROR::SHADER::FILE_NOT_SUCCESFULLY READ"<<std::endl;
    }
    const char* vertex_shader_code = vertex_code.c_str();
    const char* fragment_shader_code = fragment_code.c_str();
    
    // Shaders compilation
    unsigned int vertex;
    unsigned int fragment;
    int success;
    char infoLog[512];

    // Vertex shader
    vertex = glCreateShader(GL_VERTEX_SHADER);
    glShaderSource(vertex, 1, &vertex_shader_code, NULL);
    glCompileShader(vertex);
    glGetShaderiv(vertex, GL_COMPILE_STATUS, &success);
    if(!success) {
        glGetShaderInfoLog(vertex, 512, NULL, infoLog);
        std::cout<<"ERROR::SHADER::VERTEX::COMPILATION_FAILED \n{}"<<infoLog<<std::endl;
    }
    else {
        std::cout<<"SHADER::VERTEX::COMPILATION_COMPLETED"<<std::endl;
    }

    // Fragment shader
    fragment = glCreateShader(GL_FRAGMENT_SHADER);
    glShaderSource(fragment, 1, &fragment_shader_code, NULL);
    glCompileShader(fragment);
    glGetShaderiv(fragment, GL_COMPILE_STATUS, &success);
    if(!success) {
        glGetShaderInfoLog(fragment, 512, NULL, infoLog);
        std::cout<<"ERROR::SHADER::FRAGMENT::COMPILATION_FAILED \n{}"<<infoLog<<std::endl;
    }
    else {
        std::cout<<"SHADER::FRAGMENT::COMPILATION_COMPLETED"<<std::endl;
    }

    // Shader program
    id = glCreateProgram();
    glAttachShader(id, vertex);
    glAttachShader(id, fragment);
    glLinkProgram(id);
    glGetProgramiv(id, GL_LINK_STATUS, &success);
    if(!success) {
        glGetProgramInfoLog(id, 512, NULL, infoLog);
        std::cout<<"ERROR::SHADER::PROGRAM::LINKING_FAILED \n{}"<<infoLog<<std::endl;
    }
    else {
        std::cout<<"SHADER::PROGRAM::LINKING_COMPLETED"<<std::endl;
    }
    glDeleteShader(vertex);
    glDeleteShader(fragment);
}
```

To finally render our triangle :

```c++
Shader shader("../shaders/vertex.vs", "../shaders/fragment.fs");

while(!glfwWindowShouldClose(window)) {
    glClearColor(0.2f, 0.3f, 0.3f, 1.0f);
    glClear(GL_COLOR_BUFFER_BIT);

    shader.use();
    glBindVertexArray(VAO);
    glDrawArrays(GL_TRIANGLES, 0, 3);

    glfwSwapBuffers(window);
    glfwPollEvents();
}
```

## Element Buffer Objects

Let's say we want to draw a rectangle. Here is what we can do :

```c++
float vertices[] = {
    // first triangle
     0.5f,  0.5f, 0.0f,  // top right
     0.5f, -0.5f, 0.0f,  // bottom right
    -0.5f,  0.5f, 0.0f,  // top left 
    // second triangle
     0.5f, -0.5f, 0.0f,  // bottom right
    -0.5f, -0.5f, 0.0f,  // bottom left
    -0.5f,  0.5f, 0.0f   // top left
};
```

As you can see, we declared bottom right and top left two times each. This means that here we are using 6 vertices, where we could use only 4. Imagine that on a big model with a lot of vertices : it would be a disaster.

So what we can do is use Element Buffer Objects. We first define every vertex in our model, so in this case :

```c++
float vertices[] = {
     0.5f,  0.5f, 0.0f,  // top right
     0.5f, -0.5f, 0.0f,  // bottom right
    -0.5f, -0.5f, 0.0f,  // bottom left
    -0.5f,  0.5f, 0.0f   // top left 
};
```

And then, we store the order of the vertices to form triangles :

```c++
unsigned int indices[] = {  // note that we start from 0!
    0, 1, 3,   // first triangle
    1, 2, 3    // second triangle
};
```

We can now create the EBO.

```c++
unsigned int EBO;
glGenBuffers(1, &EBO);
```

Similar to the VBO, we bind the EBO and copy the indices into the buffer :

```c++
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, EBO);
glBufferData(GL_ELEMENT_ARRAY_BUFFER, sizeof(indices), indices, GL_STATIC_DRAW);
```

We can store the EBO inside the VAO, simply by binding the EBO and sending the data after binding the VAO, just like it was the case with the VBO.

The only difference is that now we use `glDrawElements(GL_TRIANGLES, 6, GL_UNSIGNED_INT, 0)` to draw the primitives.