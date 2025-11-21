---
base-title: 99 problems in Haskell
base-description: explanations to the solutions of 99 problems in Haskell
---

# Problem 1 - Find Last Element

```hs
myLast = foldr1 (const id)
```

- goal is to return the second argument, which is the next element
- point-free form (can omit `xs` parameter)
- `:t const :: a -> b -> a`
- `:t id :: c -> c`
- `:t const id :: b -> (c -> c)` (the first parmater is already partially applied) which becomes `:t const id :: b -> a -> a` because of currying

```hs
myLast = foldr1 (flip id)
```

```hs
myLast = foldl1 (curry snd)
```

- `:t curry :: ((a, b) -> c) -> a -> b -> c`
- `:t snd :: (a, b) -> b`
- `:t curry snd :: a -> b -> c` (partially applied the first parameter)

# Problem 2 - Find Last but One Element

```hs
myButLast (x:(_:[])) = x
myButLast (_:xs) = myButLast xs
```

- recursive solution, but with cons for pattern matching

```
myButLast = (!! 1) . reverse
```

- point-free form
- function composition f(g(x)) where g is `reverse` and f is `(!! 1)`

```hs
lastbut1 :: Foldable f => f a -> a
lastbut1 = fst . foldl (\(a,b) x -> (b,x)) (err1,err2)
  where
    err1 = error "lastbut1: Empty list"
    err2 = error "lastbut1: Singleton"
```

- requires the type annotation otherwise it is not known what type the parameter being passed in is
- says that the parameter `a` is `Foldable`

# Problem 3 - Find the kth element of a list

```hs
elementAt xs n 
  | length xs < n = error "Index out of bounds"
  | otherwise = fst . last $ zip xs [1..n] 
```

- zip up to the kth index
- note that it is not `last . zip xs [1..n]` because `zip` already has all the parameters applied, and `.` is expecting two functions as parameters

```hs
elementAt xs n = head $ foldr ($) xs 
                         $ replicate (n - 1) tail
```

- `replicate (n-1) tail` creates a list of the tail function i.e. `[tail, tail, tail, ...]`
- the accumulator starts out as the original list, subsequent folds will take the tail of the accumulators

```hs
elementAt xs n 
  | length xs < n = error "Index out of bounds"
  | otherwise = head $ drop (n - 1) xs
```

```hs
elementAt_w'pf = (last .) . take . (+1)

:t (.) :: (b -> c) -> (a -> b) -> a -> c

:t (take . (+1)) :: Int -> [a] -> [a]
:t (last .) :: (a -> [c]) -> a -> c
-- :t last :: [c] -> c
-- looking for a function that takes in a list and returns the last element

-- take . (+1) being the second function composed
-- returns a function that takes a list and returns a list
-- curried function
-- (a -> b) = Int -> ([a] -> [a])

-- last . being the first function composed
-- takes from the second function that type parameter a is [a] and c is a
-- takes in a second function, takes in a list and returns the last emement
-- curried function again
-- (b -> c) = ([a] -> [a]) -> ([a] -> a)

-- a -> c = Int -> ([a] -> a)

:t (last .) . (take . (+1)) :: Int -> [a] -> a

-- (take . (+1)) doesn't return a list so we cannot compose it was last directly
-- instead it returns a function that returns a list
-- hence when we can compose it with last . since that is looking for a function that returns a list
```
