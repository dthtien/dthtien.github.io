# Leetcode: Max area of Island

You are given an `m x n` binary matrix `grid`. An island is a group if 1's (representing land) connected 4-directionally
horizontal or vertical. You many assume all four edges of the grid are surrounded by water.

The **area** of an island is the number of cells with a value 1 in the island.
Return the maximum **area** of an island in `grid`. if there is no island, return 0

## Example
Input: grid = [
  [0,0,1,0,0,0,0,1,0,0,0,0,0],
  [0,0,0,0,0,0,0,1,1,1,0,0,0],
  [0,1,1,0,1,0,0,0,0,0,0,0,0],
  [0,1,0,0,1,1,0,0,1,0,1,0,0],
  [0,1,0,0,1,1,0,0,1,1,1,0,0],
  [0,0,0,0,0,0,0,0,0,0,1,0,0],
  [0,0,0,0,0,0,0,1,1,1,0,0,0],
  [0,0,0,0,0,0,0,1,1,0,0,0,0]
]
Output: 6
Explanation: The answer is not 11, because the island must be connected 4-directionally.

## Approach 1: Depth-first Search (recursive)
We want to know the area of each connected shape in the grid, then take the maximum of these.
if we are on a land square and explore every square connected to it 4-directionally (and recursively squares connected
  to those squares and so one), then the total number of squares explored will be of that connected shape.
To ensure we don't count squares in shape more than once, let's use `seen` to keep track of squares we haven't visited
before. It'll also prevent us from counting the same shape more than once.

```ruby
def area(grid, seen, r, c)
  return 0 if (
    r < 0 ||
    r >= grid.length ||
    c < 0 ||
    c >= grid[0].length ||
    (!seen[r].nil? && seen[r][c]) ||
    grid[r][c].zero?
  )
  seen[r] ||= []
  seen[r][c] = true

  1 + area(grid, seen, r+1, c) + area(grid, seen, r-1, c) + area(grid, seen, r, c-1) + area(grid, seen, r, c+1)
end

def max_area_of_island(grid)
  seen = [[]]
  ans = 0

  (0...grid.size).each do |r|
    (0...grid[0].size).each do |c|
      temp = area(grid, seen, r, c)
      ans = temp > ans ? temp : ans
    end
  end

  ans
end
```

## Complexity analysis
- Time complexity: O(R * C), where R is the number of rows in the given `grid` and C is the number of columns. We visit
  every square one.
- Space complexity: O(R * C), the space used by `seen` to keep track of visited squares, and the space used by the call
  stack during our recursion

## Approach 2: Depth-First search
### Intuition and Algorithm
We can try the same approach using a stack based, depth-first search
Here, `seen` will represent squares that have either been visited or are added to our list of squares to visit `stack`.
for verything starting land square that hasm't been visited, we will expore 4 directionally around it, adding land
  squares that haven't been added to seen to our `stack`.
On the side, we'll keep a count `shape` of the total number of squares seen during the exploration of this shape.
  We'll want the running max of these counts.
```ruby
def max_area_of_island(grid)
  seen = []
  ans = 0
  (0...grid.size).each do |r0|
    (0...grid[0].size).each do |c0|
      seen[r0] ||= []
      next if grid[r0][c0] != 1 || seen[r0][c0]
      shape = 0
      stack = []
      stack << [r0, c0]
      seen[r0][c0] = true
      until stack.empty? do
        r,c = stack.pop
        shape += 1
        [[r-1, c], [r+1, c], [r, c-1], [r, c+1]].each do |node|
          nr, nc = node
          seen[nr] ||= []
          if (
            nr >= 0 && nr < grid.size &&
            nc >= 0 && nc < grid.first.size &&
            grid[nr][nc] == 1 && !seen[nr][nc]
          )
            stack << [nr, nc]
            seen[nr][nc] = true
          end
        end
        ans = shape > ans ? shape : ans
      end
    end
  end

  ans
end
```

### Complexity
- Time Complexity: O(RC), where R is the number of rows in the given grid, and C is the number of columns. We visit every square once.
- Space complexity: O(RC), the space used by seen to keep track of visited squares, and the space used by stack.
