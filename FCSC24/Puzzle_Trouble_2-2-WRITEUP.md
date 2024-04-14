# FCSC24 Writeup ~ Puzzle Trouble 2/2

## Challenge Description

- **Title:** Puzzle Trouble 2/2
- **Category:** Misc
- **Difficulty:** 3/3

```
On vous demande de retrouver le flag dans ce bazar de tuiles ! Il paraît qu'elles n'ont pas été retournées...
```

## Understanding The Challenge

Similar as before we get a single image file:
```
puzzle-trouble-hard.jpg: JPEG image data, baseline, precision 8, 1024x1024, components 3
```

## Solution

Similar as before, you could solve this puzzle by hand

I decide to create a python script, `my_sol.py` to do it for me

I use `cv2` to split the image into blocks to then rearrange

Once I get `cv2` splitting the image into blocks and reconstructing it successfuly I start testing different algorithms for rearranging the blocks.

---

I do it by hand because I want points :D, and I couldn't get the algorithms to work

I create a python script to split the image into blocks, where each block is an image saved to the `blocks` directory

I drag the blocks as a collection of layers on photoshop and seperate them into similar categories

I then recursively keep dragging similar pieces closer together

Then its just like solving multiple small puzzles

I then start matching up these small solved puzzles with other nearby ones, and I iterate this process until the complete puzzle is solved

---

Once reconstructed we get:

```
This one might be harder to spot!
FCSC{93784
AE38499912
1FE3123441
984}
Puzzles for flags! 2024
```

Assemble the flag:
```FCSC{93784AE384999121FE3123441984}```