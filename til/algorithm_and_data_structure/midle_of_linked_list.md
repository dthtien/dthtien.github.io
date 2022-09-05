# LeetCode - middle of the Linked List
Given the head of a singly linked list, return the middle node of the linked list. If there are two middle nodes, return
the second middle node.

Example:

1:
1 -> 2 -> **3** -> 4 -> 5

**Input**: head = [1, 2, 3, 4, 5]
**Output**: [3, 4, 5]
**Explaination**: The middle node of the list is node 3
--------------------
2:
1 -> 2 -> 3 -> **4** -> 5 -> 6
**Input**: head = [1, 2, 3, 4, 5, 6]
**Output**: [4, 5, 6]
**Explaination**: Since the list has two middle nodes with values 3 and 4, we return the second one.
---------------------

## Approach 1: Output to Array
### Intuition and Algorithm
Put every node into an array A in order. Then the middle node is just A[A.size / 2], since we can retrive each node by
index.
```ruby
def middle_node(head)
  array = []
  while array.last.next
    array << array.last.next
  end

  array[array.size / 2]
end
```

### Complexity Analysis
- Time complexity: O(N), where N is the number of node in the given list.
- Space complexity: O(n) the space used by A.

## Approach 2: middle and end pointer
### Intuition and Algorithm
Where traversing the list with a pointer `middle`, make another pointer `end` that traverses twice as `slow`. when `end`
reaches the end of the list, `middle` must be in the middle.

```ruby
def middle_node(head)
  middle = end_node = head
  while end_node && end_node.next
    middle = middle.next
    end_mode = end_node.next.next
  end

  middle
end
```

### Complexity Analysis
- Time complexity: O(N) where N is the number of nodes in the given list.
- Space complexity: O(1), the space used by `middle` and `end`
