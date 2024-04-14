# FCSC24 Writeup ~ Tortuga

## Challenge Description

- **Title:** Tortuga
- **Category:** Misc
- **Stars:** 1

```
On vous propose une séance de dessin !
Le fichier tortuga-flag.txt contient le flag dessiné uniquement à l'aide de segments.

...

Note : le flag est de la forme FCSC{[0-9]+}.
```

## Undestanding the challenge

This challenge gives us 3 files:

```
tortuga-example.png: PNG image data, 525 x 80, 8-bit/color RGB, non-interlaced
tortuga-example.txt: ASCII text
tortuga-flag.txt:    ASCII text, with very long lines (824)
```

We need to create a script that draws vector-like images from the input provided in the flag.txt file

## Solution

I create a new python file in `subl`

I use `matplotlib` to graph the points which I represented as a `numpy` array

```python3
import matplotlib.pyplot as plt
import numpy as np

# Define the vector coordinates
vectors = [
    # Draw triangle pointing down (drawn clockwise)
    (2, 0), (-1, 2), (-1, -2),
    # Skip
    (0, 0), (3, 0),
    # Draw triangle pointing up (drawn counterclockwise)
    (-1, 2), (2, 0), (-1, -2),
    # Skip
    (0, 0), (1, 0),
] * 6

# Convert the vector coordinates to numpy arrays
vectors = np.array(vectors)

# Initialize current position
current_pos = np.array([0, 0])

skip = False
# Plotting
plt.figure(figsize=(8, 6))
for vector in vectors:

    if vector[0] == 0 and vector[1] == 0:
        skip = True
    elif skip == True:
        skip = False
        current_pos += vector
    else:
        plt.plot([current_pos[0], current_pos[0] + vector[0]], [current_pos[1], current_pos[1] + vector[1]], 'b-')
        current_pos += vector

plt.title('Flag')
plt.xlabel('X-axis')
plt.ylabel('Y-axis')
plt.axis('equal')
plt.show()
```

This works, but inthe test example the triangles are inverted due to the script not taking into account the clockwise and anti-clockwise directions

I simply add this line:

```
    vector[1] *= -1
```
below the for loop to invert the y coords.

This gives us the flag as a drawing:

` FCSC{316834725604} `