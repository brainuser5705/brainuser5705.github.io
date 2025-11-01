---
base-title: Haskell notes
description: Notes from "Learn You a Haskell"

time: Oct 24, 2025

draft: true
---

- Functional programming (what stuff is) vs Imperative programming (what to do)
- Referential transparency: function returns the same result for the same input
- Lazy: the function only runs when it needs to
```haskell

xs = [1,2,3,4]

-- Funtion goes through the whole list
doubleMe xs = [x * 2 | x <- xs] 

-- This will only make 1 pass through xs
-- Rather than calling it 3 times for every function call
doubleMe(doubleMe(doubleMe(xs)))

```
- Statically-typed: type is known as compile-time (so no runtime type errors)
- Type inference

---

- infix functions (* + / -)
- generally, prefix functions
    - we can use prefix funs as infix e.g. 92 `div` 10 -> 9
- **function application** takes the highest precedence
- functions are expressions (always return a value)
    - if/then/else, else is mandatory
- functions with no parameters are **definition/name**

--- 

# Lists

- homongenous type
- Strings are lists of characters e.g. 'a'
- lists can be compared with <, >, etc. lexiographically

Operations

- list concatenation `[a] ++ [b]`
- cons `0:[1,2,3,4] -> [0,1,2,3,4]`
    - [1,2,3] is syntactic sugar is `1:2:[3]`
- indexing `[1,2,3] !! 1 -> 2`
- head
- tail
- last - return last element
- init - `[1,2,3,4] -> [1,2,3]`
    - cannot perform these operations on empty lists
- length
- null - returns if list is empty
- reverse
- take - `take 3 [1,2,3,4] -> [1,2,3]`
    - returns whole list if the number is larger
- drop - `drop 2 [1,2,3,4] -> [4]`
    - drops whole list if number is larger
- maximum
- minimum
- sum
- product
- 4 `elem` [1,2,3,4] - return True/False

---

# Texas Ranges

- `[1..20]`
- `[2,4..20]`
- `['a'...'z']`
- `[20, 19..]`

- can take from infinite list `take 24 [13, 26..]`

- `cycle [1, 2, 3] -> [1,2,3,1,2,3..]`
- repeat 5 - infinite list of one element

---

# List Comprehension

- [x * 2 | x <- [1..10], x >= 12]
    - bounding x to 1..10 with predicate
- [x * y | x <- [1..10], y <- [1..10], x /= 1]
    - can bound to multiple lists
    - will generate all possible combinations
- nested `[[x | x <- xs, even x ] | xs <- xxs]`

---

# Tuples

- exact # of elements with exact types
- can be of different typed components
- **pair**: tuple of size 2
- cannot compare tuple of different size
- `fst` - returns the first element of pair
- `snd`
- `zip` - takes two list and zip them together into list of pairs
    - stops at the shorter list
