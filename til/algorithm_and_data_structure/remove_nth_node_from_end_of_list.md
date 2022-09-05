# LeetCode - 19. Remove Nth node from end of list
Given the head of a linked list, remove the nth node from the end of the list and return its head.

Example:
1 -> 2 -> 3 -> **4** -> 5
to
1 -> 2 -> 3 ------> 5

**Input**: head = [1, 2, 3, 4, 5], n = 2
**Output**: [1, 2, 3, 5]

## Approach 1: Two pass algorithm
### Intuition
We notice that the problem could be simply reduce to another one: remove the (L - n + 2)th node from the beginning in
the list, where L is the list length. This problem is easy to solve once we found list length L

### Algorithm
First, Find the total number of the nodes.
Second, find the index of the node that we want to remove.
Last, the linked-list node has the reference to the next node in the list. Removing the is simple by making the node before the node
we want to remove point to the node after it. In fact, we only need the node before node to remove

### Complexity
- Time complexity: O(n)
  - O(2n):
    - Iterate once to find length.
    - Iterate again to remove nth node.
Space complexity: O(1)

```ruby

def remove_nth_from_end(head, n)
  # find the length of list
  size = 0
  current_node = head

  while current_node
    current_node = current_node
    size += 1
  end

  # handle the case that remove first node
  return head.next if size == n

  # Find node to remove
  node_before_removed_index = length - n - 1;
  node_before_removed_index.times do
    current_node = current_node.next
  end

  current_node.next = current_node.next.next
  head
end
```

## Approach 2: one pass algorithm
### Algorithm
The above algorithm could be optimized to one pass. Instead of one pointer, we could use two pointers. The first
pointer advances the list by n + 1 steps from the beginning, while the second pointer starts from the beginning of the
list. Now, both pointers are exactly separated by n nodes apart. We maintain this constant gap by advancing both
pointers together until the first pointer arrives pass the last node. The second pointer will be pointing at the nth
node counting from the last. We relink the next pointer of the node referenced by the second pointer to point to the
node's nex next node

## Complexity

- Time complexity: O(L)
The algorithm makes one traversal of the list of L nodes. Therefore time complexity is O(L)
- Space complexity: O(1): we only use constant extra space

```ruby
def remove_nth_from_end(head, n)
  # move current_node n steps into list
  current_node = head
  n.times { current_node = current_node.next }

  return head.next unless current_node

  # Move both pointers until current_node reaches the end of list
  node_before_removed = head
  while current_node.next
    current_node = current_node.next
    node_before_removed = node_before_removed.next
  end
  node_before_removed.next = node_before_removed.next.next

  head
end
```
