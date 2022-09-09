# 21 - Merge two sorted lists

You are given the heads of two sorted linked lists `list1` and `list2`.
Merge the two lists in a one sorted list. The list should be made by splicing together the nodes of the first two lists.
return the head of the merged linked list.

## Example
**Input**: list1 = [1, 2, 4] list2 = [1, 3, 4]
**Output** [1, 1, 2, 3, 4, 4]


## Approach 1: Recursion
### Intuition
We can recursively define the result of a `merge` operation on two lists as the following (advoiding the conrner case
logic surrounding empty list):
Namely, the smaller of the two lists' heads plus the result of a `merge` on the rest of the elements.

### Algorithm
We model the above recurrence directly, first accounting for edge cases. Speciafically, if either of `li` or `l2` is
initially `null`, there is no merge to perform, so we simply return the non -`null` list. otherwise, we determine which
of `li` and `l2` has a smaller head, and recursively setr the `next` value for the head to the next merge result. Given
that both lists are `null` - terminated, the recursion will eventually terminate.

```ruby
def merge_two_lists(list1, list2)
  return list2 unless list1
  return list1 unless list2

  if list1.val < list2.val
    list1.next = merge_two_lists(list1.next, list2)
    list1
  else
    list2.next = merge_two_lists(list1, list2.next)
    list2
  end
end
```

### Complexity Analysis
- Time complexity: O(n+m): Because each recursive call increments the pointer to `list1` or `list2` by one( approaching
  the dangling `null` at the end of each list), there will be exactly one call to `merge_two_lists`/ element in each list.
  Therefore, the time complexity is linear in the combined size of the list.
- Space complexity: O(n + m)
The first call to `merge_two_lists` does not return until the ends of both `list1` and `list2`  have been reached so n +
m stack frames consume O(n + m) space

## Approach 2: Iteration

### Intuition
We can achive the same idea via iteration by assuming that `list` is entirely less than `list2` and processing the
elements one by one, inserting elements of `list2` in the necessary place in `list1`

### Algorithm
First, we set up a false `prehead` note that allows us to easily return the head of the merged list later. We also
maintain a `prev` pointer, which points to the current node for which we are considering adjusting its `next` pointer.
Then, we do the following until at least one of `list2` and `list1` point to `null`: if the value at `list1` is less than
or equal to the value at `list2`, then we connect `list1` to the previous node and increament `list2`. Otherwise, we do
the same, but for `list2`. Then, regardless of which list we connected, we increment `prev` to keep one step behide one
of our list heads.
After the loop terminates, at most one of `list1` and `list2` is non - `null`. Therefore - because the input lists were
in sorted order - if either list is non - `null`, it contains only elements greater than all of the previously-merged
elements. This means that we can simply connect the non-null list to the merge list and return it
```ruby
def merge_two_lists(list1, list2)
  head = ListNode.new
  tail = head

  while list1 && list2 do
    if list1.val < list2.val
      tail.next = list1
      list1 = list1.next
    else
      tail.next = list2
      list2 = list2.next
    end

    tail = tail.next
  end

  tail.next = list1 || list2

  head.next
end
```

### Complexity

- Time complexity: O(m + n)
Because exactly one if `list1` and `list2` is incremented on each loop interation, the `while` loop runs for a number of
iterations equal to the sum of the lengths of the two lists. All other work is constant so the overall complexity is
linear.
- Space complexity: O(1)
The iterative approach only allocates a few pointers, so it has a constant overall memory footprint


