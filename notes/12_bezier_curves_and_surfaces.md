# Bezier curves

- More complex curves by increasing degree
  - But: *quasi*-local influence of each control point means minor changes to
    one point *do* influence nearby segments -> Lack of fine control over shape

## Spline curves

- Originated in ship-building, planks along the hull of the ship
- PS: Spline curve is not polynomial, just a concatenation of multiple
  polynomials!
- For n-degreee polynomials: 1st to (n-1)-th differentials must be equal at
  connecting points
  - (n-1) in order to not lose all DOF for second curve (one DOF left for)

### Piecewise bezier

- If Cn continuity desired, then moving one control point will move the `n`
  following control points too

## Quiz

### Polynomial curves

- All polynomial bases have equal approxmation power (same functional room)

### Bezier curves

- Cn smoothness: All polynomials infinitely smooth (infinite number of derivations)
- Local control: Only pseudo-local control!

### Bezier curves 2

- All control point linear -> convex hull zero -> has to be line
- Part of circle not possible for bezier curves, rationality is missing
- Higher-degree polygon leading to identical curve: In monome form, polynome of
  degree n can be represented with polynome of degree n + k

# Freeform surfaces

## Bilinear interpolation

- (1-u)(1-v)b0 + (1-u)vb1 + ...
- bi: Control points

## Tensor product surfaces

- Move one curve along another curve -> results in surface
- nB: Tensor product in linear algebra is outer product which increases
  dimension, eg tensor product of two vectors is 2d matrix

- Evaluation with de casteljau: For curves it was a triangular structure, for
  surfaces a pyramide-like

## B-spline surface

- N_i^m: Base
