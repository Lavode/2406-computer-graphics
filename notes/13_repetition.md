# Outro

## Colors

## Ray-tracer

### Intersections

- Good exam question: New object with implicit representation, figure out
  intersection points

### Shadows

- Extremely easy to add, surpress lighting components if in shadow
  (intersection algorithms already in place)

### Spatial data structures

- Bounding volumes: Nested, eg spheres within spheres within .... -> higher efficiency / fewer intersection tests

## Rasterization

### Transformations / Projections

- Good exam questions: Transformations
- Exam question: Coordinate conversions (world, object, ...)

- GPU: Extremely fast at matrix/vector operations -> As much as possible via those

### Rasterization

- Vectors to pixels
- Specifically: Rasterization of straight lines (Bresenham)
  - Good from discrete POV
  - Exam question

### Visibility

- Z-Buffer: Memory hungry (less of an issue with modern hardware), limited resolution
