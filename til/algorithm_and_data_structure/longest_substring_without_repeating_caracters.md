# Leetcode 3. Longest substring without repeating characters

Given a string `s`, find the length of the **longest substring** without repeating characters.

Example:
**Input**: s = 'abcabcbb'
**Output**: 3
**Explanation**: The answer is `abc`, with the length of 3

## Overview
The primary challenge in this problem is to find an efficient way to get all possible longest substrings that contain no
duplicate characters. To achieve this, we need to take advantages of Hash Table, which checks if a character occurs
before quickly.
In the following three appoaches, we utilize a has table to guarantee substrings have no repeating characters and
optimize the algorithm to query possible substring step by step:

## Approach 1: Brute force

### Intuition
Check all the sustring one by one to see if it has no duplicate character

### Algorithm
Suppose we have a function `all_unique?(string)` which will return true if the characters in substring are all unique,
otherwise false. We can iterate through all the possible substrings of the given string `s` and call the function
`all_unique?`. If it turns out to be true, then we update our answer of the maximum length of substring without
duplicate characters.

Let's fill the missing parts:
  - To enumerate all substrings of a given string, we enumerate the start and end incices of them. Suppose the start and
    end indices are i and j respectively. Then we have 0 <= i < j <= n. Thus, using two nested loops with i from 0 to n - 1 and j from i + 1 to n we can enummerate all substrings of `s`.
  - To check if one string has duplicate characters, we can use a set. We iterate through all the characters in the
    string and put them into the set one by one. Before putting one character. We check if the set already contains it.
    If so, we return `false` after the loop, we return true

### Implementation

```ruby
def length_of_longest_substring(s)
  s_size = s.size
  return 0 if s_size.zero?

  length = 1
  s_size.times do |i|
    j = i + 1
    string = s[i]
    while j < s_size
      cr = s[j]
      break if string.include? cr

      string += cr
      curent_size = j - i + 1
      length = length > current_size ? length : current_size

      j += 1
    end
  end

  length
end
```

### Complexity
 - Time complexity: O(n^3)
    to verify if characters within index ranges(i, j) are all unique, we need to scan all of them. Thus, it cost O(j - i)
time.
    For a given i, the sum of the costed by each j in [i + 1, n] is the ∑O(j - i). Thus the sum of all time consumtion is
O(n^3)
  - Space complexity: O(min(n, m)). We need O(k) space for checking a substring has no duplicate characters, where k is
    the size of the `string`. The size of the `string` is upper bounded by the size of the string n and the size of the
    charset m

## Approach 2: Sliding window
### Intuition:
Given a subsring with a fixed end index in the string, maintain a HashMap to record the frequency of each character in
the current sustring. If any characers occurs more than once, drop the leftmost characters until there are no duplicate
characters.

### Algorithm

The naive approach is very straightforward. But it too slow.
In the naive approaches, we repeatdly check a substring to see iof it has duplicate character. But it's unnecessary. if
a string sij frm index to j-1 is already checked to have no duplicate characters. We only need to check if s[j] is
already in the substring sij.
To check if a character is already in the substring, we can scan the substring, which leeds to an O(n^2) algorithm. But
we can do better.

By using Hash as a sliding window, check if a character in the current can be done O[1].

A sliding window is an abstract concept commonly used in array/string problems. A window is a rang of elements in
array/string which usually defined by the strart and end indices, I.E [i, j) (left-closed, right open). A sliding window
is a window `slides` its two aboundaries to the certain direction. For example, if we slide [i, j)to the right by 1
element, then if becomes [i+1, j +1].

Back to our problem. We use Hash to store the characters in current window[i, j)(j = i initially). Then we side the
index j to the right. If it;s not in the hash, we slide j further. Doing so until s[j] is already in the Hash. At this
point, we found the maximum size of substrings, without duplicate characters start with index i. if we do this for all
i, we get our answer.

### Implementation
```ruby
def length_of_longest_substring(s)
  chars = Hash.new(0)
  left = right = 0
  result = 0
  while right < s.size
    r = s[right]
    chars[r] += 1

    while chars[r] > 1
      l = s[left]
      chars[l] -= 1
      left += 1
    end
    current_length =  right - left + 1
    result = result > current_length ? result : current_length

    right += 1

  end

  result
end
```

### Complexity Analysis
- Time complexity: O(2n) = O(n). In the wrost case each character will be visited twice by i and j
- Space compexity: O(min(n, m)). Same with the previous approach. We need O(k) space for the sliding window, we K is the
  size of the `Hash`. The size of the `Hash` is upper bounded by the size of the string n and the size of the charset m.

## Approach 3: Sliding window optimized

### Intuition

The above solution requires at most 2n steps. In fact, it could be optimized to require only n steps. Instead of using a
set to tell if a character exists or not, we could define a mapping of the characters to is's index, then we can skip
the characters imediatly when we found a repeated character.

### Algorithm

The reason is that if s[j] have a duplicate in the range [i, j) with index j', we don't need to increase i little by
little. we can skip all the elements in the range [i, j'] and let i = j' + 1 directly

```ruby
def length_of_longest_substring(s)
 n = s.size
  result = 0
  hash = {}

  i = 0

  n.times do |j|
    if hash.key? s[j]
      position = hash[s[j]]
      i = i > position ? i : position
    end

    current_length = j - i + 1
    result = result > current_length ? result : current_length
    hash[s[j]] = j + 1
  end

  result
end
```
