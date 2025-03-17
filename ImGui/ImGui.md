Quick Dear ImGui C++ Cheatsheet
# ImGui Cheatsheet

## Setup and Frame Lifecycle

#### With OpenGL

```cpp
#include "imgui.h"
#include "imgui_impl_glfw.h"
#include "imgui_impl_opengl3.h"
#include <GLFW/glfw3.h>

// Create context and initialize style

ImGui::CreateContext();
ImGui::StyleColorsDark();

// Initialize Platform/Renderer backends (example using GLFW and OpenGL3)

// You need to set an appropriate GLSL version (e.g., "#version 130")

ImGui_ImplGlfw_InitForOpenGL(window, true);
ImGui_ImplOpenGL3_Init(glsl_version);

// At the start of each frame:
ImGui_ImplOpenGL3_NewFrame();
ImGui_ImplGlfw_NewFrame();
ImGui::NewFrame();

// ... build your UI here ...

// At the end of each frame:

ImGui::Render();
ImGui_ImplOpenGL3_RenderDrawData(ImGui::GetDrawData());

## Basic Window

ImGui::Begin("My Window");   // Create a window and start adding widgets

// Widgets go here:

ImGui::Text("Hello, world!");
ImGui::Button("Click Me");
ImGui::End();                // Always call End()!
```
#### With SDL

```cpp
#include "imgui.h"
#include "imgui_impl_sdl.h"
#include "imgui_impl_sdlrenderer.h"
#include <SDL2/SDL.h>

// Create context and initialize style

ImGui::CreateContext();
ImGui::StyleColorsDark();

// Initialize Platform/Renderer backends

ImGui_ImplSDL2_InitForSDLRenderer(window, renderer);
ImGui_ImplSDLRenderer_Init(renderer);

// At the start of each frame

ImGui_ImplSDL2_NewFrame();     // If using SDL2 backend
ImGui_ImplSDLRenderer_NewFrame(); // If using SDL_Renderer backend
ImGui::NewFrame();

// ... build your UI here ...

// At the end of each frame:

ImGui::Render();
ImGui_ImplSDLRenderer_RenderDrawData(ImGui::GetDrawData());

## Basic Window

ImGui::Begin("My Window");   // Create a window and start adding widgets

// Widgets go here:

ImGui::Text("Hello, world!");
ImGui::Button("Click Me");
ImGui::End();                // Always call End()!
```
## Common Widgets

### Text & Labels

```cpp
ImGui::Text("This is some text.");
ImGui::LabelText("Label", "Value: %d", 10);
```
### Buttons

```cpp
if (ImGui::Button("Button Label")) {
    // Button pressed action
}
```
### Checkboxes

```cpp
bool checked = false;
ImGui::Checkbox("Checkbox Label", &checked);
```
### Sliders

```cpp
float value = 0.0f;
ImGui::SliderFloat("Slider Label", &value, 0.0f, 1.0f);
```
### Input Fields

```cpp
char buf[128] = "";
ImGui::InputText("Input", buf, sizeof(buf));
```
### Combo Boxes

```cpp
const char* items[] = { "Item 1", "Item 2", "Item 3" };

int current_item = 0;

ImGui::Combo("Combo Box", &current_item, items, IM_ARRAYSIZE(items));
```
### Color Editor

```cpp
float color[3] = { 0.2f, 0.3f, 0.4f };
ImGui::ColorEdit3("Color Editor", color);
```
## Layout Commands

```cpp
// Separator
ImGui::Separator();

//Spacing
ImGui::Spacing();
    
//Same Line
ImGui::SameLine();
```
## Tree and Collapsing Headers

```cpp
if (ImGui::CollapsingHeader("Section Header")) {

    ImGui::Text("Content under header");

}

if (ImGui::TreeNode("Tree Node")) {

    ImGui::Text("Inside tree node");

    ImGui::TreePop();

}
```
## Popups and Modal Dialogs

```cpp
if (ImGui::BeginPopupModal("Modal Window", NULL, ImGuiWindowFlags_AlwaysAutoResize)) {

    ImGui::Text("Modal content here");

    if (ImGui::Button("Close")) {

        ImGui::CloseCurrentPopup();

    }

    ImGui::EndPopup();

}
```
### Cleanup

#### With OpenGL

```cpp
ImGui_ImplOpenGL3_Shutdown();
ImGui_ImplGlfw_Shutdown();
ImGui::DestroyContext();
```
#### With SDL

```cpp
ImGui_ImplSDLRenderer_Shutdown();
ImGui_ImplSDL2_Shutdown();
ImGui::DestroyContext();
```