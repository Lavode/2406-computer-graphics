# Triangle meshes

## Triangle meshes

- Allow rendering arbitrary shapes made out of (or approximated by) triangles.
- Approximation of piecewise linear stuff (eg circumference of circle): Error
  shrinks with n^2 (n number of vertices)
- General approximation: Error shrinks linearly with # of vertices
- Adaptive meshing: More triangles in places with high curvature, few (big)
  triangles in 'flat' places.

## Vertices / Edges / faces

- Genus 0: Closed object, no holes (eg coffee cup is not genus 0)
- In infinite mesh:
  - Each edge connected to two faces

## Storing meshes

### Face set

- For each triangle, coordinates of vertices stored
  - 9 * 32 bit numbers per triangle

### Indexed face set

- Vertice coordinates in dedicated table, with index per vertex
- Face table references vertices by indices
- Comparably compact => Good for storage
- Not ideal to traverse mesh => Bad for processing
  - Given vertex n, find neighbours => have to traverse face list, store
    indices => traverse vertex list, store coordinates => remove duplicates

### Face-based connectivity

- Vertex table stores: Position per vertex, and reference to one of three connected faces
- Face table stores: Three vertices, and three neighbouring faces

### Edge-based connectivity

- Lack of orientation makes traversal complicated

### Half-edge based connectivity

- Vertex stores: One of either outbound or inbound edge (normally outbound, but
  can be implemented either way)
- Face stores: Reference to one of its half-edges
- Edge stores:
  - Next / previous edge of face

## Ray tracing meshes

- Issue: Each triangle is planar -> same surface normal -> triangles visible
  - Mach band effect ('edge detector' in brain) emphasizes it even more

### Optimization


- k-DOPs:
  - Generalization of OBB, box where corners are cut off
  - 2D: Polygon, 3D: Polytope => Approximating object

- Hierarchical bounding volumes:
  - Each bounding volume in exactly one bounding volume, and each bounding
    volume exactly one object

## Quiz

### Light paths

- Standard raytracer able to handle: E[S*](D|G)L
