---
base-title: Haskell notes (Higher order functions)
base-description:
time: Oct 30, 2025
---

# Higher Order Functions

- functions that are *parameters* and *return values*

- every function only takes **one** parameter
- all functions that accept multiple parameters are **curried functions**

```haskell
max 4 5
(max 4) 5

-- creates a function that takes a parameter and returns 4 or that parameter
-- then we pass in 5 to that function
```

- `->` in the type definition, or putting a space, is the **function application**

```haskell
max :: (Ord a) => a -> a -> a
-- same as
max :: (Ord a) => a -> (a -> a)
```

- curried functions are made up of **partially applied functions**
- function that takes in as many parameters as we left out
- good for create "on-the-fly" functions

```haskell
-- compareWithHundred :: (Num a, Ord a) => a -> Ordering
compareWithHundred x = compare 100 x
compareWithHundred = compare 100
```

- infix functions are *partially applied* using *sections*
- surround with parentheses and supply parameter on one side, it will create a function that takes another parameter to apply on the other side

```haskell
divideByTen = (/10)
-- dividebyTen 200 = 200 / 10

isUpperAlphanum = (`elem` ['A'..'Z'])
```

> But sections don't work with - since `(-4)` means negative 4. So we have to use `(subtract 4)`

- cannot display partially applied functions
- functions are not part of the `Show` typeclass

---

- functions as parameters

```haskell
-- require the parentheses to signify the parameter is a function
applyTwice :: (a -> a) -> a -> a
applyTwice f x = f (f x)
```

```haskell
flip' :: (a -> b -> c) -> (b -> a -> c)
flip' f = g
    where g x y = f y x

flip' :: (a -> b -> c) -> b -> a -> c
flip' f x y = f y x
```

- function application is right associative (applied right to left)
- we can take advantage of currying when making higher-order functions by thinking ahead and writing what their end result would be if they were called fully applied

---

## Maps and Filters

`map` applies function to every element in the list

```haskell
map :: (a -> b) -> [a] -> [b]
map _ [] = []
map f (x:xs) = f x : map f xs
```

`filter` keeps elements only if they satisfy a predicate

```haskell
filter :: (a -> Bool) -> [a] -> [a]
filter _ [] = []
filter p (x:xs) =
    | p x = x: filter p xs
    | otherwise = filter p xs
```

- can use list comprehension for `map` and `filter`
    - `filter` with multiple predicate - use `&&`

- Haskell's lazy programming: mapping and filtering a list several times, will still only pass over the list once

---

## Lambdas

- are expressions
- anonymous functions to be used only once
- (\parameters -> function body)

```haskell
filter (\xs -> length xs > 15) (map collatzChain [1..100])
```

- partial application > lambdas
- pattern match in the parameters of lambdas

```haskell
map (\(a,b) -> a + b) [(1,2), (3,5), (6,3)]
```

- curried functions can be read easier 

```haskell
addThree :: (Num a) => a -> a -> a -> a
addThree x y z = x + y + z
addThree \x -> \y -> \z -> x + y + z -- illustrates currying
```

---

## Folds

- like `map` but reduces to a single value
- takes:
    - binary function
    - starting value (accumulator)
    - list to fold up
- binary function is called with accumulator and first/last element to produce a new accumulator
- good for traversing through list element by element

### `foldl`

- left fold
- folds the list up from the left side (head value)

```haskell
sum' :: (Num a) => [a] -> a  
sum' xs = foldl (\acc x -> acc + x) 0 xs  
-- x is the head of xs

-- using currying in mind
sum' = foldl (+) 0 
```

- currying: if you have a function like `foo a = bar b a` - can rewrite it as `foo = bar b`

### `foldr`

- right fold
- folds the list from the right side (tail value)
- current value is the first parameter, accumulator is the second parameter `\x acc -> ...`

- `foldr` works with infinite lists, but not `foldl`

### `foldl1` and `foldr1`

- assume that first or last element of list is the starting vlaue
- requires at least one element

### `scanl` and `scanr`

- put the intermediate accumulator values in a list
- `scanl1` and `scanr1`
    - final results is on the side of the fold

---

## Function application

- with spaces, function application is left-associative `f a b c` -> `((f a) b) c)`
- **lowest precedence** of any operator
- saves key strokes

```haskell
sum (map sqrt [1..130])
-- vs
sum $ map sqrt [1..130])
```

```haskell
-- (4+9) plus the sqrt of 3
sqrt 3 + 4 + 9
-- vs sqrt of 3 + 4 + 9
sqrt $ 3 + 4 + 9
```

```haskell
sum (filter (> 10) (map (*2) [2..10]))
-- vs
sum $ filter (> 10) $ map (*2) [2..10]
```

- can map function application over a list of functions

```haskell
map ($ 3) [(4+), (10*), (^2), sqrt]
```
---

## Function composition

- `.` operator
- right associative `(f (g (z x)))` = `f . g . x`

```haskell
(.) :: (b -> c) -> (a -> b) -> a -> c  
f . g = \x -> f (g x)  
```

```haskell
map (\x -> negate (abs x)) [5,-3,-6,7,-3,2,-19,24]
-- vs
map (negate . abs) [5,-3,-6,7,-3,2,-19,24]
```

- can only take one parameter at a time, so for functions that require more than one parameter...

```haskell
sum (replicate 5 (max 6.7 8.9))
-- need to be written as 
(sum . replicate 5 . max 6.7) 8.9
sum . replicate 5 . max 6.7 $ 8.9
```

- if expression ends in 3 parentheses, then it will need 3 composition operators

- writing in **point free style**




