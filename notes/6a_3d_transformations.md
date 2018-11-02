# 3D Transformations

- Scaling and translation: Analogous to 2D, with one additional coordinate

## 3D rotation

- Rotation: In 3D in the general case not commutative
  - Note: In 2D, rotation counter-clockwise (right-hand rule), so positive angle -> counter clockwise
    - Default rotation matrix ((cos a, -sin a), (sin a, cos a))
      - cos in diagonals, since if angle 0 column vectors = base vectors
- Euler angles: If R(a, b, c) = Rx(a) * Ry(b) * Rz(c) => Read right-to-left, due to matrix
  multiplication
  - Note: First rotation (z axis) affects other axis, so second rotation around
    transformed y axis
  - Gimbal lock: Certain positions are degenerated, two axes parallel -> loss
    of one degree of freedom in that position

- Rodrigues' rotation formula
  - nB, with n matrix: n^T * n => scalar, n * n^T => matrix

- Magic formula:
  - R*n = n, since n ortagonal to plane
  - Downside: If wanting to combine multiple rotations -> Hard to combine
    - Beter: Convert to plain matrix (Euler) form, combine, convert  back

## Coordinate transformations

- Model -> World coordinates: Eg dimensions mm vs m, model-local coords vs global coords, ...
- Depth range: Camera does not see from right in front of camera to infinity,
  has min/max distance
- Normalized device coordinates: Already close to hardware, all transformd to
  2x2x2 cube. (Coordinates in [-1, 1]^3)

### Camera

- One of view, up, right vector not needed to fix camera, but can be used for
  completness' sake
- All coordinates using world coordinates

### View transformation

- Note that transformation matrix potentially made out of multiple transormations (eg translation, rotation, ...)
  - When calculating inverse: Operations have to be reversed too
  - Split into individual operations, eg A = T * R, then invert individually

## Projections

- Planar: Project on flat plane
  - Non-planar: Eg projection on sphere
- Orthographic: View rays perpendicular to plane
  - Oblique: Not perpendicular ;)

### OpenGL orthographic

- Standard says: Camera looks along negative z axis (PS)
  - Object closest to camera have biggest z coordinates
  - As closest objects should have smallest z coordinates, we mirror those
- Scale viewing box to OpenGL 2x2x2 view
