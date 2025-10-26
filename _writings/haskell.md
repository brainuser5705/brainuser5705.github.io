---
base-title: Haskell notes
description: Notes from "Learn You a Haskell"

time: Oct 24, 2025

draft: true
---

# Intro

- What is the diff betwen functional and imperative programming?
    - Functional programming (what stuff is)
    - Imperative programming (what to do)
- What is **Referential transparency**?
    - function returns the same result for the same input
- What does it mean for Haskell to be **Lazy**?
    - the function only runs when it needs to

    ```haskell
    xs = [1,2,3,4]

    -- Function goes through the whole list
    doubleMe xs = [x * 2 | x <- xs] 

    doubleMe(doubleMe(doubleMe(xs)))
    -- This will only make 1 pass through xs
    -- Rather than calling it 3 times for every function call
    ```

- What does **statically-typed** mean?
    - types is known as compile-time (so no runtime type errors)
- Type inference

---

# Basic Functions

- infix functions (* + / -)
- prefix functions
    - use as infix function e.g. 92 `div` 10 -> 9

- **Function application** takes highest precedence
- Functions are **expressions** - always return a value
- Functions with no parameters are **definition/name**

--- 

# Lists

- Types of values must be the same
- `String`s are lists of characters e.g. 'a'
- Lists can be compared with <, >, etc. lexiographically

## Operations

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

```haskell
[1..20]
[2,4..20] -- step
['a'...'z']
[20, 19..] -- reverse
```

- can `take` from infinite list `take 24 [13, 26..]`
- `cycle [1, 2, 3]` -> `[1,2,3,1,2,3..]`
- `repeat 5` - infinite list of one element

---

# List Comprehension

- [x * 2 | x <- [1..10], x >= 12]
    - bounding `x` to `1..10` with predicate
- [x * y | x <- [1..10], y <- [1..10], x /= 1]
    - can bound to multiple lists
    - will generate all possible combinations
- nested list comprehension: `[[x | x <- xs, even x ] | xs <- xxs]`

---

# Tuples

- Type of the tuple = exact # of elements with exact types
- Can be of different typed components
- **pair**: tuple of size 2
- Cannot compare tuple of different size
- `fst` - returns the first element of pair
- `snd`
- `zip` - takes two list and zip them together into list of pairs
    - stops at the shorter list

---

