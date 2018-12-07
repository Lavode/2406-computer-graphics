# Bezier curves

- Freeform curves: Must be smooth, implementation via eg spatial points would
  lead to 'jaggy' curve

- Note: Figuring out equations for x/y/z coordinates to achieve desired form is
  *hard*. Hence alternative approach: Bezier curves.

## Bernstein polynomials

- Maxima of functions of given degree are distributed 'evenly' along [0, 1]
  interval

- Partition of unity: Weights add to one => geometric meaning

## Bezier curves

- Convex hull: Connect all points with each other, biggest possible enclosed
  area is convex hull
  - Curve being within convex hull ensures limitation of curve extents
- Pseud-local control: Allows intuitive modifications
  - In monomial base: Changing one parameter can and will influence whole curve

### De Casteljau

- For each value in interval:
  - n control points -> blend to create n-1 control points
  - Repeat
  - Once only one control point reached: Point on curve
