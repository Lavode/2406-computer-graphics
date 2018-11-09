# Texturing and Shadowing

## Texturing

- Allows additional details at bareley any performance impact
- Each vertex (in 3D!) assigned (u, v) coordinates on texture
  - Interpolation inbetween

### Texturing one triangle

- Artefacts since interpolation constants not linear
- 'perspective' mode, which is correct for most cases, is default in OpenGL

### Simple parametrizations

- Certain distortion due to type of projection, additional distortion if object
  does not match shape which projection is intended for

- For navigational purposes: Projections which preserve angles rather than area

### Low distortion projections

- First example: Artefacts when lines orthogonal to image plane, good projection if parallel to plane

### Texture interpolation

- Potentially:
  - One fragment covers many pixel texturese
  - One pixel of texture covers many fragments

### Aliasing

- "White curves" which are orthogonal to actual black curves
- Caused by: One pixel in image plane (cone) might cover many pixels
  - Sampling the point in the middle of the cone is wrong -> 'random' value
    shown on plane
  - Realistically: Average of all covered pixels should be used, but expensive => EWA filtering
  - Approximative:
    - Mipmapping: Multiple resolution textures, select pixel from texture where pixel size ~= fragment size
    - General: Results have fewer artefacts, but become blurry

### Special texture maps

- Light map: Only for static scenes, or scenes with finite number of stati, as
  pre-computed
- Spherical environment map:
  - Take picture of scene reflected in sphere
  - Use picture to then render reflecting objects in same scene
- Various maps: Allow real-time lifelike renderings

## Shadow in rasterization

- In raytracer it was easy: For each point, check if path to light source
  - For rasterization less straight-forward
- Z Buffer: Visibility from the POV of the camera. Shadow buffer: Visibility from the POV of the light source

### Shadow buffer

- With (x, y) of image plane pixel, and z buffer (ie distance from plane), we
  can transform back into world coordinates (inverse of MVW transformation)
- Self-shadowing easy, unlike with other algorithms
- Artefacts: Shadows have pixel-y structure due to resolution of shadow map
  - Smooth shadows would require high-resolution shadow map
  - Projection aliasing: Not easily solvable without increasing resolution of
    shadow map
  - Perspective shadow map: Instead of using world coordinates, use camera
    perspective
    - Requires rebuilding shadow map when camera moves, but more accurate
      shadows
    - Excluding rebuild cost, not much more costly - only a different matrix
      for transformation - in dynamic scenes, shadow map has to be rebuilt
      every frame anyway, so no additional cost.
- Hard shadows due to assumption that light source point-shaped (infinitely
  small) -> doesn't match reality
