# CUDA Ray Tracer

A GPU-accelerated ray tracing renderer utilizing CUDA and OpenGL for high-performance 3D rendering.

---

## Overview

This project implements a real-time ray tracer that leverages **CUDA** for parallel computation of ray intersections and shading. **OpenGL** and **GLFW** are used for window management and rendering display. It currently supports basic geometric primitives (spheres), with plans for more complex geometry.

---

## Features

* CUDA-accelerated ray tracing for fast 3D rendering
* Real-time interactive camera controls
* OpenGL integration for efficient image display
* Cross-platform windowing with GLFW
* Shader-based post-processing pipeline

---

## Requirements

* NVIDIA GPU with CUDA support
* CUDA Toolkit 10.0 or newer
* OpenGL 4.3+
* [GLEW](http://glew.sourceforge.net/)
* [GLFW3](https://www.glfw.org/)
* [GLM](https://github.com/g-truc/glm)

---

## Architecture Overview

### CPU (Host)

* GLFW/OpenGL for window & context management
* Camera input and scene control
* Scene setup and data transmission to GPU

### GPU (Device)

* Ray generation from camera
* Parallel ray-object intersection tests
* Shading calculations
* Frame buffer population for rendering

---

## Build Instructions

### Windows (Visual Studio)

1. Install CUDA Toolkit and compatible Visual Studio version
2. Open the `.sln` file in Visual Studio
3. Configure CUDA and library paths in project settings
4. Build the solution

### Linux

```bash
mkdir build && cd build
cmake ..
make
```

---

## Usage

### Controls

* `W/A/S/D` - Move camera
* `Mouse` - Look around
* `Esc` - Exit

---

## Rendering Pipeline

### Ray Generation

* Camera generates primary rays per pixel
* Rays calculated using perspective projection

### Parallel Ray Processing

* CUDA kernel launches one thread per pixel
* 2D thread grid matches image resolution

### Intersection Testing

* Ray-sphere intersection via quadratic formula
* Closest hit and normal are recorded

### Shading

* Basic Lambertian diffuse shading
* Lighting intensity based on angle of incidence

### Frame Buffer Update

* Colors written to frame buffer in parallel
* Memory transferred from GPU to CPU

### OpenGL Display

* Frame buffer mapped to OpenGL texture
* Displayed on screen-aligned quad via GLFW

---

## Technical Implementation

### Vector Math

#### CUDA Vector3

* Optimized for GPU with `__host__` / `__device__` qualifiers
* Supports standard vector operations
* Used in device-side math calculations

#### GLM Integration

* Used on CPU for OpenGL and camera math
* Compatible with OpenGL matrix pipeline

---

### Camera System

* First-person camera with pitch/yaw rotation
* View matrix calculated from forward/right/up vectors
* Ray direction derived from screen-space to world-space mapping

---

### Ray-Sphere Intersection

* Uses analytical geometry to find intersection points
* Handles rays inside objects
* Early exits and minimal sqrt usage for performance

---

## CUDA Integration

* Each kernel thread maps to one screen pixel
* Memory coalescing and pinned memory for efficiency
* Scene data in constant memory
* Minimal host-device transfers per frame

---

## OpenGL Display

* Frame buffer texture updated each frame
* Quad rendered with basic shaders
* VSync handled via GLFW

---

## Performance Considerations

* Full GPU parallelization (1 thread per ray)
* Memory coalescing and fast math used
* Shared memory & constant memory where applicable
* CUDA stream used for overlapping rendering/display/input
* Profiling available via CUDA events

---

## Current Limitations

* Only supports spheres
* No shadows, reflections, or global illumination
* Vector math bugs (e.g., incorrect `-=` operator)
* Camera model lacks depth of field or motion blur
* Fixed frame buffer size
* No denoising or advanced materials

---

## Future Enhancements

### Geometry

* Ray-triangle intersection
* OBJ/FBX model support
* Scene graph with transformations

### Acceleration Structures

* BVH, kd-tree, or octree
* GPU-optimized traversal algorithms

### Advanced Rendering

* Path tracing & photon mapping
* PBR material system (BSDF)
* Textures: UV, normal, environment mapping

### Performance

* AI-based denoising
* Multi-GPU rendering
* RTX and DXR support

### User Experience

* GUI-based scene editor
* Post-processing (bloom, DOF, tonemapping)
* Export high-res images and video

---

## License

This project is licensed under the Apache 2.0 License – see the [LICENSE](LICENSE) file for details.

---

## Acknowledgments

* [GLFW](https://www.glfw.org/) – Windowing and input
* [GLM](https://github.com/g-truc/glm) – Vector/matrix math
* [GLEW](http://glew.sourceforge.net/) – OpenGL extensions
