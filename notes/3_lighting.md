# Lighting

## Quiz
- Pinhole camera:
  - Infinitely small hole ensures that every ray reflected off of
    light source is on exactly one point on image.
    - Bigger hole will cause multiple image points - but also more light -> more bright
  - Size on image plane depends not only on object size, but also on angle
    between camera and object, as image plane is... plane
  - The bigger distance between hole and image plane, the smaller field of view of image
- Vectors perpendicular: Dot product = 0
- Degrees of freedom of plane: Three 
  - (x^2+y^2+z^2 = 0)
  - Or: n^T * x - d = 0
    - n is normalized => Two degrees of freedom
    - d scalar => One dof
    - => three dof
- Point / plane distance (Plane: Normal n, Center c, Point x): 
  - <x - c, n>
- Barycentric coordinates: ...

## Importance of lighting

- No lighting leads to lack of depth information
- Local illumination: Reflection of light sources on surfaces
- Global illumination: Mirroring, etc

## Surface reflectance

- Direct or indirect reflections of light rays emitted by light source
- Light hitting our eye described as integral, with parameters:
  - L_in: Incoming light for given angle
  - f: Influence of material: BRDF, bi-directional reflectance distribution function

### Simplifications for raytracing

- Ray tracing approximates integral via three directions => Light is lost
- Phong lighting approximates BRDF, also compensating for lost light
  - Ambient light: Compensates for lost light
  - Diffuse: For eg dull materials (paper, cloth, ...)
  - Specular: High-gloss (metal, mirror, ...)

## Ambient lighting

- Basic light-level of scene

## Diffuse reflection

- Specific light sources hitting object => Even distribution in all directions.
  - Typical for dull materials (cloth, paper)
  - Light intensity depends on angle at which source hits object
- Assumption: Objects are filled => No light reflections within object

## Specular reflection

- Assuming perfect reflection, inbound light rays reflected, with same
  outbound angle
- Angle between camera and reflected ray influences resulting light too
- PS: Calculation of vectors as per slides
- Calculation of vector s:
  - <n, l> is projection of l onto n
  - Multiplied by n to get vector
- One 'light cone' for each light source hitting the object

### Shininess

- s = 1: Big resulting 'light cones'
- s high (eg 200): Small, well-differentiated light cones


## Colours

- Theoretically reflected light depends on wavelength => Lots of integrals
- Approximation: Only three spectral components, RGB

## Notation:

- a * b, a, b (three-dimensional) vectors: (a1 * b1, a2 * b2, a3 * b3)
- a x b, cross product.
  - Easy to remember: Write two vectors next to each other, and swapped below each other.
  - Start with cross of lower two components of upper vectors
  - Magic!
