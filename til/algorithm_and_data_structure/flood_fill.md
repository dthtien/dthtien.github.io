# Flood fill

An image is represented by an `m x n` integer grid `image` where `image[i][j]` represents the pixel value of the image.

You are also given three integers `sr`, `sc`, and `color`. You should perform a **flood fill** on the image starting
from the pixel `image[sr][sc]`.
To perform a **flood fill**, consider the starting pixel, plus any pixels connected **4 driectionally** to the starting
pixel of the same color as the starting pixel, plus any pixels connected **4-directionall** to those pixels (also with
the same color), and so on. Replace the color of all of the aforementioned pixels with `color`.

return the mdified image after performingthe flood fill.

Example:
**Input**
[
  [1, 1, 1],
  [1, 1, 0],
  [1, 0, 1],
]
**Output**
[
  [2, 2, 2],
  [2, 2, 0],
  [2, 0, 1],
]

**Explanation**: From the center of the image with position(sr, sc) = (1, 1), all pixels connected by a path of the same
color as the starting pixel (i.e., the blue pixels) are colored with the new color.
Note the bottom corner is not coloered 2, beause it's not 4 directionally connected to the starting pixel.


# Approach #1. Depth-first search [Accepted]

## Intuition
We perform the algorithm explained in the problem description: paint the starting pixels, plus adjacent pixels of the
same color and so on.

## Algorithm
Say `color` is the color of the starting pixel. Let's floodfill the starting pixel: we change the color of that pixel to
the new color, then check the 4 neighboring pixels to make sure they are valid pixels of the same color, and of the
valid ones, we floodfill those and so on.
We can use a function `dfs` to perform a floodfill on a target pixel

```ruby
def dfs(r, c, image, color, cl)
  br = image.size
  bc = image.first.size

  return if image[r][c] != cl

  image[r][c] = color
  dfs(r-1, c, image, color, cl) if r >= 1
  dfs(r+1, c, image, color, cl) if r + 1 < br
  dfs(r, c -1, image, color, cl) if c >= 1
  dfs(r, c+1, image, color, cl) if c + 1 < bc
end

def flood_fill(image, sr, sc, color)
  cl = image[sr][sc]
  return image if cl == color


  dfs(sr, sc, image, color, cl)
  image
end
```
