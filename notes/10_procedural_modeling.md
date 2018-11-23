# Procecdural modeling

- L-Systems: Modeling of plants
- Generic procedural modeling: Textures (eg stone, wood, ...), etc

## Solid textures

- Wood: Volumetric, so can't just map texture cube-like, from all sides
- Separate pieces: Eg broken off, in a way such that seams match

## Photography

- For proper texture capturing (including normal and other maps): Many pictures
  with lots of different light colours and angles required

## Procedural

### Noise functions

- Will be added to function which describes to-be-generated shape, introduced
  randomness makes it more 'natural'

- Examples: LHS -> 1D, RHS -> 2D with same technique

- Frequencec limitation: No noise in-between grid points, since noise values
  chosen for each grid point, with interpolation inbetween
  => Grid layout directly influences frequency

#### Issues

- Interpolation: `d` in 'nearest lattice values' is number of dimensions, eg
  linear in 3D -> 2^3 = 8 values

### 2D Perlin noise

- Define function via:
  - Values at grid points = 0
  - Differential at each grid point
- Linear interpolation would produce artefacts -> Smooth interpolation function
  - After halfway-point between two grid points: Influence of closer point
    grows faster than if linear interpolation used
  - Polynomial of degree 5, can also use another depending on implementation
    needs
- Scalar product is linear function in each point, for calculation of value of
  noise function in said point

### Modified perlin noise

- Polynomial of degree 5 (as per above), original used degree 3
  Only 12 pre-defined gradients, as vision not very susceptible to changes in
  direction (as opposed to eg colour)

### Example: Marble

- x, y, z: 3D coordinates
- If no noise function: Simple white/black/white gradients

### Self-similarity

- fBm: Statistical, ie if looking at sufficiently large sub-areas, average
  value and frequency will always be comparable

### Logarithmic spiral

- x(t) = a*cos(t), y(t) = a*sin(t) would be a circle, with e^(bt) it becomes
  (depending on choice of b) the given spiral

### Fractals

- Formal definition: Non-integer dimension. Eg 1d = curve, 2d = area,
  fractal-curve "somewhere in-between"

### fBm

- Homogenous: Everywhere we are it looks the same
- Isotrope: In one point, everywhere I look it looks the same
