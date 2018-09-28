# Raytracing

- Goal: Generating realistic digital image based on 3D scene
- Assumptions: 
  - Light propagates along rays, not waves
    - No diffraction, interference, or polarization
  - Approximate light spectrum with discrete RGB wavelengths
    - No dispersion or fluorescencec
  - No atmospheric scattering or gravity effects
  - Multiple light sources add linearly

## Pinhole camera

- Adaption for rendering:
  - Pinhole = Position of eye
  - Image plane = In front of eye
  - Advantange: Not mirrored

## Forward ray tracing

- Light sources generate lots of rays
- Rays reflect off of surfaces (recursive computation until nth reflection)
- If ray hits eye -> Increase brightness of respective pixel of plane
- Computationally expensive

## Backward ray tracing

- Eye sends out rays
- Follow rays plus reflections thereof
- If ray hits light source -> Increment brightness of respective pixel of image plane
  - Amount of light depends on how 'direct' connection between object and light source is
- Primary ray: Eye -> Object -> Light source
- Secondary ray: Eye -> Object -> Object -> Light source

## Ray tracing pipeline

- Three stages:
  - Ray generation: How to represent rays mathematically
  - Ray intersection: How to calculate intersections
  - Lighting: How to translate interesections to brightness

### Ray generation

- r(t) = o + t * d; o 3D-Point, t number, o 3D-Vector

- Primary rays:
  - Origin o = eye point
  - 3D position of pixel: Generally center of pixel used
  - Direction normed, to simplify further calculations

### Ray intersection

- Geometric primitives:

#### Unit sphere

- perfectly spherical
- radius 1
- Description as:
  - (Phi, Theta) -> (x, y, z):
    - Theta in [-pi/2, pi/2] (latitude)
    - Phi in [-pi/2, pi/2]. longitude
    - Downside: Intersection with rays is complicated and non-linear, due to
      trigonometric functions
  - (|| x - c || - r = 0)
    - r radius in R
    - c center in R^3
    - x point on sphere in R^3

##### Intersection with ray

- || o + td - c || = r
- ...
- t^2 * d^T * d + 2 * t * d^T * (o - c) + (o - c)^T * (o - c) = r^2
- Assume b = 2d^T(o-c), c = (o-c)^T(o-c)-r^2
- Equation of form at^2 + bt + c => Solve quadratic equation => (Up to) two
  solutions for intersection

#### Planes

- Implicit equation:
  - n normal, in R^3 (orthogonal to plane)
  - c point, in R^3, at base of n
  - *or* d distance, in R^3, from (0,0,0)

##### Intersection with ray

- n^T * (o + td) - d' = 0

#### Triangles

- Barycentric coordinates: 'All points are equal', no special treatement

