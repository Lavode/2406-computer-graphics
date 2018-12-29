# Overview

- Computer graphics: 3D scene / model -> 2D image
- Computer vision: 2D image -> 3D scene / model
- Image processing: 2D image -> 2D image
- Geometry processing: 3D scene / model -> 3D scene / model


# Raytracing

- Assume:
  - Light propagates along rays, not waves
    - No diffraction, interference, and polarization
  - Approximate light with discrete wavelengths
    - No dispersion, no fluorescence
  - Superposition
  - No participating medium:
    - no atmospheric scattering

## Ray

- Ray generation, ray intersection, lighting
- Explicit: r(t) = o + t * d
- Primary ray: From origin through each pixel of image plane

## Sphere

- Explicit representation via polar coordinates (phi, theta)
  - Radius r, center c, point on sphere characterized as:
    - p(phi, theta) = c + r * (cos(phi) * cos(theta), sin(phi) * cos(theta), sin(theta))

- Implicit representation: abs(x - c) - r = 0
- Intersection with ray: abs(o + t*d - c) - r = 0

## Plane

- Implicit representation:
  - Normal n, center (starting point of normal) c:
    - n . (x - c) = 0
  - Alternatively with d (scalar!) distance from origin:
    - n . x - d = 0

- Intersection: n^T(o + t * d) - d = 0
  - t = (d - n^T * o) / (n^T . d)
  - If t exists, intersection (t positive: in front of ray, t negative: behind ray origin = "wrong" intersection)

## Products

- SPICK
- a = (a1, a2, a3), b = (b1, b2, b3), c = a x b = (a2b3 - a3b2, a3b1 - a1b3, a1b2 - a2b1)
  - C is orthogonal to a and b, abs(c) is area between a and b, abs(c) = abs(a x b) = abs(a) * abs(b) * sin(theta)
- vol(a, b, c) = (a x b)^T . c

## Triangle

- Barycentric coordinates, A, B, C points of triangles: x = alpha * A + beta * B + gamma * C, with:
  - alpha + beta + gamma = 1, alpha, beta, gamma >= 0 (if later condition lacking: plane rather than triangle)

- Explicit

- Intersection:
  - o + t * d = alpha * A + beta * B + gamma * C, ANDA alpha + beta + gamma = 1
  - solve 3x3 system
    - if 0 lteq alpha, beta, gamma lt 1, then intersection inside triangle, else outside

## Light

- Light leaving point x: Integrate over all inbound angles, light_in * cos(theta_in) * brdf(light_in, light_out)
  - BRDF: bidirectional reflectance distribution function, material properties (eg selectiv reflectance in certain angles, ...)
- Raytracing approximation: Three angles
  - From point directly towards light source
  - Perfect reflection
  - Perfect refraction
- Phong lighting:
  - Ambient: Approx global light transport, uniform in, uniform out
  - Diffuse: Dull/matt surfaces, directed in, uniform out
  - Specular: Shiny surfaces, directed in, directed out

### Ambient

- Global
- I = I_a * m_a
  - I_a: Ambient light intensity (and ...)
  - m_a: Ambient reflection coefficient

### Diffuse

- Per light-source
- Directed in, ie directed light-ray inbound. Angle at which it hits point
  determines light brightness! (compare sun / earth)
- I = I_l * m_d * cos(theta)
  - I_l: Light source intensity (and colour and ...)
  - m_d: Material's diffuse reflection coefficient
  - Theta: Angle between light ray and **surface normal**
- If n (normal), l (point-to-light vector) normalized: cos(theta) = n . l
  - I = I_l * m_d * (n . l)
  - n . l lt 0 implies light "from other side" of point, hence no lighting

### Specular

- Per light-source
- n: Normal
- SPICK
- l: Point-to-light vector, normalized, r = 2 * n * (n . l) - l: Light reflected at normal. (Also origin at point)
- v: Point-to-eye vector, normalized
- Theta: Angle between normal and l = between normal and r
- Alpha: Angle between Point-to-eye vector and r
- s: Shininess
  - s large: Small, bright spots. S small: Large, dark spots
- m_s: Material's specular reflection coefficient
- I = I_l * m_s * cos(alpha)^s = I_l * m_s * (r . v)^s
  - n . l lt 0 implies light from other side, so no illumination
  - r . v lt 0 implies reflected ray will not reach eye, so no illumination

### Colour

- Approximated with RGB components
- 3D colour vector, component-wise addition/multiplication in equations above

### Shadow

- Discard diffuse & specular components, if point-to-light-source vector
  intersects another object
  - Shadow acne: Either offset origin of shadow ray along positive surface
    normal ("outside" of object), or ignore intersections too close to the
    object, or offset along point-to-light-source vector

### Recursive tracing

- For reflection and/or refraction
- Reflection: out = (I - 2nn^T) in = in - 2n(n^T in)
- Refraction:  n1 * sin(theta1) = n2 * sin(theta2), with n1, n2 refraction
  indices
- Resulting ray: Process as usual. Interpolate recursively calculated light with
  light from main iteration based on material properties (eg "reflectivity")

# Triangle meshes

- Vertices V ({v_i, in R^3}), Edges ({e_i, in V^2}), Faces ({f_i, in V^3})
- Any shape can be approximated, error inversely proportionate to number of vertices (even with n^2 if piecewise linear)
- SPICK
- #F ~= 2 * #V, #E ~= 3 * #V
- Meshes of genus 0 (closed, no holes, eg coffee cup is not): Euler's formula: V - E + F = 2

## Face set

- For each face, store coordinates of its three vertices
- With 4-byte float: 9 coordinates per face = 36 B/F = 72 B/V

## Indexed face set

- One table with coordinates of all vertices
- One table with faces, using indices
- 12 B/B + 12B/F = 36 B/V
- Compact, good for storage
- Not good for traversal/processing, have to traverse face list, store indices,
  traverse vertex list, store coordinates, remove duplicates

## Face-based connectivity

- Vertex table: Position per vertex, reference to one of its faces
- Face table: Ref to three vertices, ref to three neighbouring faces
- 16 B/V + 24 B/F = 64 B/V
- Edges not represented

## Edge-based connectivity

- Vertex table: Position, ref to one of its edges
- Face table: Ref to one of its edges
- Edge table: Ref to its two vertices, ref to its two faces, ref to the two 'next' and the two 'prev' edges of its faces
  - No edge orientation requires case distinction during traversal
- 16 B/V + 4 B/F + 32 B/E = 120 B/V

## Half-edge based connectivity

- Vertex table: Position, ref to either outbound or inbound edge (per-implementation)
- Face table: Ref to one of its half-edges
- Edge table: Ref to one of its vertices, ref to its (sole!) face, ref to next, prev, and opposite half-edges
  - No case distinction during traversal
- 16 B/V + 4 B/F + 20 B/H = 144 B/V (can be reduced to 96 B/V due to redundancy)
- Ring traversal:
  - Start at V, Outgoing, Opposite, Next, Opposit, Next, Opposite, ...

# Shading

## Flat shading

- Constant (per-face) normal for lighting
- Facetted appearance

## Phong shading

- SPICK
- Normal per vertex
- For each pixel on face: Barycentric interpolation of normal vectors, weighted
  by area, or opening angle w(T_i)
- Triangle T_i (a,b,c), triangle normal: n(T_i) = ((b - a) x (c - a)) / abs((b - a) x (c - a))
  - Vertex normal: n(V) = sum(w(T_i) * n(T_i)) / abs(sum(w(T_i) * n(T_i)))
- Interpolated normal of x in triangle (A, B, C): n(x) = alpha * n(A) + beta * n(B) + gamma * n(C)
  - Don't forget to normalize

## Gouraud shading

- SPICK
- Light per vertex, then directly interpolated (rather than normal)

# Mesh intersection

- Brute force: O(n), n triangles

## Spatial subdivision

- Allow cutting down on the number of required ray-triangle intersections
- KD trees: Even splits (coordinate-based), or surface heuristics (area-based), or ...
- Top-down construction
- Selects objects based on given volumes
- O(log(n))


## Bounding volume hierarchies

- Sphere: Complex intersection
- AABB:
  - Extremely easy to check intersection
  - Unnecessarily large if objects not axis-aligned (eg diagonally spread)
- Oriented bounding box (OBB):
  - Like AABB, but rotated
  - Reasonably easy to check intersection
  - Unnecessarily large if objects sparse
- k-DOPs:
  - OBB, with n edges cut off
- General: Complex shape -> tight fit but complicated intersection. Basic shape -> easy intersection but loose fit
- Construction: Bottom-up
- Traversal: Top-down
- Selects volume based on given objects
- O(log(n))

# Light paths

- E Eye, L Light, D diffuse, S specular
- Paths:
  - LE
  - LDE
  - LSE
  - LDDE
  - LSDE
  - LDSE

- Standard raytracer can handle: `E[S*](D|G)L`
  - No multiple diffuse-inter-reflections (eg EDDL)
  - No caustic (eg EDSL)

# Rasterization

- Forward rendering
- Projects 3D objects onto 2D plane
- Pipeline optimized for hardware implementation (eg parallelization)

- Low-level rendering / rendering commands: OpenGL, Direct3D, Vulkan
- High level scene graphs (scene description): OpenInventor, Java3D, OpenSceneGraph, ...
- Window API: Glue between OpenGL (or other low-level components), and windowing system: Qt, GLUT, GLFW, ...

## OpenGL

- Primitives:
  - GL_POINTS (Points)
  - GL_LINES (Lines)
  - GL_LINE_STRIP (Connected lines)
  - GL_LINE_LOOP (Connected, looping, lines)
  - GL_TRIANGLES (Doh)
  - GL_TRIANGLE_FAN (Triangles all sharing one vertex, and pairwise sharing one edge)
  - GL_TRIANGLE_STRIP (Triangles pairwise sharing one edge)

- Vertex arrays (VAs): Store vertex data
- Vertex buffer objects (VBOs): Store VAs, stored on the GPU
- Vertex array objects (VA)s): Store VBOs

### Vertex shader

- Input: Vertex attributes, constant parameters
- Output: 
  - Required: Vertex coordinates in "clip space", after MVP transformation (model view projection)
  - Optional: Vertex colour, texture coordinates, point size, ...
- Interpolated outputs are sent to fragment shader
- One vertex in, one vertex out, no connectivity information!
- Todo: MVP transformation, normal transformation, per-vertex lighting (for Gouraud)
- Not possible: Clipping, perspective division, viewport transformation

### Fragment shader

- Input: Fragment window position, colours, texture coordinates, phong model parameters if used, constant parameters
    - Most is interpolated from output of fragment shader
- Output: Pixel's colour, optionally pixel's depth value
- TODO: Texture fetch & application, per-pixel lighting (if used)
- Not possible: Alpha blending, per-fragment operations

# 2D Transformations

- SPICK: Linear maps: Matrix - vector product with matrix columns being linear transformations of unit/basis vectors
- Note: Linear mappings have to preserve origin, L(0) = L * 0 = 0
  - Translations do *not* preserve the origin: T(0) = (t_x, t_y)
  - Translations are **affine** transformations. **Affine** mapping = linear mapping + translation:
    - (x, y) -> ((a, b), (c, d)) * (x, y) + (t_x, t_y) = Lx + t

## Homogenous coordinates

- Additional coordinate w: 1 = point, 0 = vector
  - Vector + vector = vector
  - Point - point = vector
  - Point + vector = point
  - Point + point = nonsense

## Translation

- SPICK
Matrix representation: M =
```
1  0  t_x
0  1  t_y
0  0  1
```
With P = (x, y, 1) point in homogenous coordinates: MP = (x + t_x, y + t_y, 1)

## Scaling

- SPICK
```
s_x  0    0
0    s_y  0
0    0    1
```

## Rotation

- SPICK
```
cos(a)  -sin(a)   0
sin(a)   cos(a)   0
0        0        1
```

- SPICK
- Note: Columns of matrix are images of basis vectors!
  - With Point in polar coordinates: r = (x, y), phi (angle):
    - Rotation: (x, y) -> (r cos(phi + theta), r sin(phi + theta))
    - NB: a * cos(b + c) = a * cos(b) cos(c) - a * sin(b) sin(c)
    - And a * sin(b + c) = a * cos(b) sin(c) + r sin(b) cos(c)

## Concatenation of transformations

- Given A_1, ..., A_n affine transformations: A_n(...(A_1(x))) = A_n ... A_2 A_1 * x
  - Able to precompute matrix one time, and apply many transformations at once to a point

## Important questions

- Straight edges stay straight: Every point on line AB is affine combination alpha A + (1-alpha) B
  - Affine transformations preserve those: M(alpha A + (1-alpha) B) = alpha M(A) + (1-alpha) M(B)

# 3D transformations

- SPICK

## Translation

```
1  0  0  t_x
0  1  0  t_y
0  0  1  t_z
0  0  0  1
```

## Scaling

```
s_x  0    0    0
0    s_y  0    0
0    0    s_z  0
0    0    0    1
```

## Rotation

Rx:

```
1    0      0       0
0    cos(a) -sin(a) 0
0    sin(a)  cos(a) 0
0    0      0       1
```

Ry:

```
 cos(a)  0  sin(a)  0
 0       1  0       0
-sin(a)  0  cos(a)  0
 0       0  0       1
```

Rz:
```
cos(a)  -sin(a)  0  0
sin(a)   cos(a)  0  0
0        0       1  0
0        0       0  1
```

### Rodrigues' rotation formula

- Alternative to euler angles above
- Rotation by angle theta around normalized axis n

```
R(n, theta) = nn^T + cos(theta)(I - nn^T) + sin(theta) * N
```

With N:
```
0     -n_z  n_y
n_z   0     -n_z
-n_y  n_x   0
```

# 3D projections

- SPICK
- Split 3D -> 2D transformation into multiple, smaller, easier, steps
- Model coords -- model transformation -- > world coordinates -- view transformation -- > camera/eye coordinates -- projection transformation --> normalized device coordinates -- viewport transformation --> window/pixel coordinates

## Model transformation

- Place model in world
- In: Model coordinates (local to model)
- Out: World coordinates (global)

## View transformation

- Setup extrinsic camera parameters, position & orientation
- In: World coordinates (global)
- Out: Camera / eye coordinates (standard camera)

- Camera location (e), viewing direction (v), up direction (u), right direction (e)
- v, u, r orthonormal
- One of v, u, r not needed to fix camera, but can be specified
- Standard camera:
  - Location 0,0,0
  - Viewing direction 0, 0, -1
  - Up 0, 1, 0
  - Right 1, 0, 0

From standard camera to e, r, u, v:
```
rx  ux  -vx  ex
ry  uy  -vy  ey
rz  yz  -vz  ez
0   0   0    1
```

- Note that transformation matrix potentially made out of multiple transormations (eg translation, rotation, ...)
  - When calculating inverse: Operations have to be reversed too
  - Split into individual operations, eg A = T * R, then invert individually

From e, r, u v to standard camera: use inverse thereof:
```
rx  ry  rz  0     1  0  0  -ex
ux  uy  uz  0  *  0  1  0  -ey
-vx -vy -vz 0     0  0  1  -ez
0   0   0   1     0  0  0  1
```

## Projection transformation

- Setup intrinsic camera parameters, opening angle and depth range
- In: Camera / eye coordinates
- Out: Normalized device coordinates ([-1, 1]^3)

- SPICK
- Parallel projections:
  - Preserve parallellism & lengths
  - technical applications
- Perspective projections:
  - Each point projects toward center of projection
  - Realistic
  - Graphics applications

- Extend homogeous coordinates: w = 0 implies vector, w in R != 0 implies point

### Orthographic projection

- Special case of parallel, with viewing direction perpendicular to image plane
- If onto xy plane: Simply remove z coordinate
   - Analogous for other planes

### Generic perspective projection

- With image plane at z = -d

```
1  0  0     0
0  1  0     0
0  0  1     0
0  0  -1/d  0
```

## Viewport transformation

- Setup image parameters/resolution, width and height
- In: Normalized device coordinates ([-1, 1]^3)
- Out: Pixel coordinates [l, l+w] x [b, b+h] x [0, 1]

```
w/2  0    0    w/2 + l
0    h/2  0    h/2 + b
0    0    1/2  1/2
0    0    0    1
```

# Rasterization in detail

- Affine transformations preserve affine combinations:
  - Vertex meshes can be transformed / projected / ... by simply transforming /
    projecting each vertex.

## Transformation of normal vectors

- With transformation matrix M, T(n) = M^(-T) * n

## Line rasterization

- y = m * x + b

### Naive

- For each horizontal pixel x: y = round(m * x + b), draw(x, y)
- Multipliciation (potentially costly, worsens rounding errors) and rounding (further rounding errors)

### Digital differential analysis

- Start with x = 0 (or offset), y = b
- For each horizontal pixel x: y = round(y + m), draw(x, y)
- No multiplication, but still rounding, as y and m are floats

### Bresenham

- SPICK
```
dx = x1 - x0
dy = y1 - y0
d = 2 * dy - dx // 
de = 2*dy  // Move east
dne = 2 * (dy - dx) // Move north-east

draw(x0, y0)
for (x = x0, y = y0; x <= x1;) {
  if (d <= 0) { d += de; x++ }
  else { d += dne; x++; y++}
  draw(x, y)
}
```

- Only integer arithmetics
  - Good for systems without FPU
  - Fast
  - No accumulating rounding errors

## Visibility

### Painter's algorithm

- Sort polygons with regards to farthest vertex (farthest first)
- Check if there are overlapping z ranges
- Paint from back to front
- O(n * log(n)) for n polygons

#### Disambiguation step

- SPICK
- If zmax(A) > zmax(B) > zmin(A) (otherwise no need to do anything):
- Paint A if:
  - A's and B's [x, y] ranges do not overlap or
  - A is behind B's supporting plane or
  - B is in front of A's supporting plane or
  - Projections do not overlap
- Swap A and B if:
  - B is behind A's supporting plane or
  - A is in front of B's supporting plane
- Repeated swaps indicate cyclic overlap:
  - Split A into two polygons at B's supporting plane (or vice versa)

### Z-Buffer

- SPICK
- After MVP & viewport transformation: Store current minimal Z value of each pixel
  - Framebuffer for RGB colours, Z buffer for depth values (additional 16-32 bits per pixel)
    - No issue on modern hardware

- Z-buffer initialized to infinity
- Rasterize triangles, interpolate depth value per fragment
- With `z` z value of current fragment
```
if z < zbuffer(x, y) {
  // Current fragment is closer to camera than previously-drawn pixels at (x, y)
  framebuffer(x, y) = rgb;
  zbuffer(x, y) = z;
}
```
- O(n) for n polygons
- nB: Higher resolution near near plane
