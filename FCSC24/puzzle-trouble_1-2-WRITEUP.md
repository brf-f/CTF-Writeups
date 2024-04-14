# FCSC24 Writeup ~ Puzzle Trouble 1/2

## Challenge Description

- **Title:** Puzzle Trouble 1/2
- **Category:** Misc
- **Difficulty:** 1

```
On vous demande de retrouver le flag dans ce bazar de tuiles ! Il paraît qu'elles n'ont pas été retournées...
```

## Understanding the Challenge

This challenge gives us 1 file:
```puzzle-trouble.jpg: JPEG image data, baseline, precision 8, 1024x1024, components 3```

Looking at the image we see it consists of tiles with parts of letters and numbers.

I think we have to reconstruct the tiles like a puzzle

## Solution

I simply decide to use photoshop to reconstruct the flag

After splitting up the individual blocks and reconstructing I get 3 sections

```
FCSC{12
83AEBF1
9387475}
```

putting them together gives the flag

`FCSC{1283AEBF19387475}`