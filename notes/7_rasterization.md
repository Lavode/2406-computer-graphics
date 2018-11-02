# Rasterization

- Recap: 
  - Raytracing -> One ray through each pixel. "Pixels first"
  - Rasterization -> Triangles generate pixels. "Triangles first"

- Idea: For each triangle, transform edges -> Transformed triangle identical to
  if each point on triangle had been transformed individually.

## Projective transformation

- Note: Parameter alpha might change, ie middle of original line is not middle
  of projected line

## Normal transformation

- n^T x = 0, equation of three-dimensional plane (4 dimensions coordinate system)
- Effect happens with all transformations which stretch one (or multiple) axis
  - Not with eg rotations, see also: R^(-T) = R^(-1)^T = R^T^T = R, for its transformation matrix

## Rasterization

## Drawing

### Painter's algorithm

- IRL: Painting back-to-front allows to paint over boundaries, rather than
  having artefacts where one line a bit too long/short

- Use fact that camera axis-aligned -> [x, y] coordinate range of two objects not overlapping guarantees no overlapping of objects

- Recognizing cycles is hard, and splitting up is ugly -> algorithm not used a lot

### Z Buffer

- Downside in early times: One additional float per pixel has to be stored
- Now: Multiple GB of memory per card -> irrelevant
- O(n) since not a full sort, only one value per pixel stored

## Quiz

- Z fighting due to float inaccuracies -> both visible
- Location of near-plane can (due to varying z resolution) cause artefacting

