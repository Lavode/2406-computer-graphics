# Fractals / L-System

- So far: How to render scenes on screen
- Now: How to generate scenes, modeling, procedural, ...

## L-system

- Parallel (as many as possible) execution of rules -> Fewer results possible
- Turtle graphics: Commands for turtle can be encoded in string -> Generation
  via L system
- NB: ccw = counter-clockwise, cw = clockwise
- 'X': Used to 'remember' node if eg resulting graphic not perfectly
  symmetrical, eg nested triangles

### L-systems with pen & paper

- Mind that, when eg replacing F with production rule, that existing rotation
  has to be respected - if line already rotated by -90, this will be added to
  new rotations introduced by production rule

### Quiz

#### Forward problem

- Note: Overlapping structure -> Rotations must not cancel -> Definitely not A

#### Inverse: Regular grid

- No need to visit every line only once -> overlapping is fine, makes rules easier
  - Allows to stay local with rules

### L-system rules

- Branching structures:
  - `[` stores current position & rotation on stack
  - `]` restores topmost position & rotation from stack
