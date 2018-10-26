# Rasterization

- Rasterization much faster than raytracing => Good for real-time rendering (eg computer games)

## Rasterization pipeline

- Layout (what operation on what data) optimized for hardware implementation
- Projection: Parallel vs perspective
  - Parallel relevant for technial renderings, eg architecture -> Lengths and angles not (or reliably) distorted
- Rasterization: Transformation of vector world to pixel world
- Shading:
  - Gouraud: Light per vertex, and then light directly interpolated (rather
    than normal, in the case of Phong)
- Visibility: Eg via Z buffering, for each pixel, keep track of depth
  - New object: Only render at pixel (x,y), if object's depth is lower than stored depth for (x, y)

## Computer graphics APIs

- APIs for direct rendering: OpenGL (cross-platform), Direct3D (Windows), Vulkan (cross-platform, new), Metal (Mac OS)
- APIs for high level scene graphs: OpenInventory, Java3D, OpenSG, ...

## OpenGL

- 2.0: Introduction of shaders -> Possible to replace steps of pipeline with
  custom ones

### Primitives

- Additional primitives (strip, fan, ...) help with caching, as vertices shared

### Vertex shared

- Vertex-based - works on one vertex at a time, no information about others.
- Some things not possible in shader, hard-coded in OpenGL standard (ie on GPU)
  - Perspective division: Transformation of 3d to 2d coordinates
