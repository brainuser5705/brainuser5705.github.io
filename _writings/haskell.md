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

---

# Types and Typeclasses

- `:t` - to get the type of an expression
- `::` type of 
- explicit **type declaration** for functions
```haskell
echo :: [Char] -> [Char]
echo str = str
```
- multiple parameters type declaration
```haskell
add' :: Num -> Num -> Num
```
- type variables to represent any type
    - for **polymorphic functions**
```haskell
head :: [a] -> a
-- takes a list of values of some type and returns a value of the same type
```

- `Int` bounded by min and max value
- `Integer` no bounds
- `Float`
- `Double` double precision
- `Bool`
- `Char`
- () empty tuple

- If a type is part of a *typeclass* then it has to satisfy some methods of the typeclass *interface*
```haskell
(==) :: (Eq a) => a -> a -> bool
-- class constraint (Eq a), a must be member of Eq class
```
- Members of `Eq` must implement functions `==` and `/=`

- `Ord` implements >, <, >=, <=
```haskell
compare :: (Ord a) => a -> a -> Ordering
-- Ordering is a type with values GT, LT, EQ
```

- `Show`
    - members implement `show` which presents value as a string
- `Read`
    -`read` takes a string and returns a type
    - need a type annotation to explicitly tell it what type to return
```haskell
read :: (Read a) -> String -> a
read "4" -- this would cause errors because Haskell doesn't know what type
read "4" :: Int
```

```
- `Enum`
    - `succ` and `pred`
- `Bounded`
    - minBound :: Int
    - maxBound :: char
- `Num`
    - Integer, Int, Float, Double
    - `10 :: Double` ->  `10.0`
- `Integral`
    - Integer, Int
- `Floating`
    - Float, Double

- `fromIntegral`
    - converts Integeral to more general `Num`
```haskell
fromIntegral(length [1,2,3]) + 3.0
- need to convert the Integer to be same type as 3.0
```

---

## Pattern Matching

- top to bottom matching (base case on top)
- if none of the patterns match, then error
- `(x:xs), (x:y:ys), (x:y:_)` binding for lists
    - `(x:[])` is same as `(x:y:[])` 

- `error String -> Runtime error` function

