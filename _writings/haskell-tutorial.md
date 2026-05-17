---
base-title: Haskell Notes
base-description: Notes from the book "Learn You a Haskell for Great Good!" by Miran Lipovača.

draft: true
---

# Table of Contents
- [Features of Haskell](#intro)
- [Lists](#list)
    - Operations
    - Texas Ranges
    - List Comprehension
    - Tuples
- [Function Syntax](#syntax)
    - Pattern Matching
    - Guards
    - `where`
    - `let`
- [Higher Order Function](#hof)
    - Maps and Filters
    - Lambdas
    - Folds
    - Function application
    - Function composition
- [Module](#module)
- [Alegbraic Data Types](#algebra)
    - Record Syntax
    - Type Parameters
    - Derived Instances
    - Type Synonyms

---

<a id="intro"></a>

# Section 1: Features of Haskell

- Functional programming (what stuff is) vs Imperative programming (what to do)
- **Referential transparency**: function returns the same result for the same input
- **Statically-typed**: type is known as compile-time (so no runtime type errors)
- Type inference
- **Lazy**: the function only runs when it needs to
    ```haskell
    xs = [1,2,3,4]

    -- Funtion goes through the whole list
    doubleMe xs = [x * 2 | x <- xs] 

    -- This will only make 1 pass through xs
    -- Rather than calling it 3 times for every function call
    doubleMe(doubleMe(doubleMe(xs)))
    ```

- infix functions (* + / -)
- can use prefix functions as infix e.g. 92 `div` 10 -> 9
- **function application** takes the highest precedence
- functions are expressions (always return a value)
- **definition/name**: functions with no parameters

--- 

<a id="list"></a>

# Section 2: Lists

- homongenous type
- Strings are lists of characters e.g. 'a'
- lists can be compared with <, >, etc. lexiographically

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

## Texas Ranges

- `[1..20]`
- `[2,4..20]`
- `['a'...'z']`
- `[20, 19..]`

- can take from infinite list `take 24 [13, 26..]`

- `cycle [1, 2, 3] -> [1,2,3,1,2,3..]`
- repeat 5 - infinite list of one element

## List Comprehension

- [x * 2 | x <- [1..10], x >= 12]
    - bounding x to 1..10 with predicate
- [x * y | x <- [1..10], y <- [1..10], x /= 1]
    - can bound to multiple lists
    - will generate all possible combinations
- nested `[[x | x <- xs, even x ] | xs <- xxs]`

## Tuples

- can be of different typed components
- **pair**: tuple of size 2
- cannot compare tuple of different size
- `fst` - returns the first element of pair
- `snd`
- `zip` - takes two list and zip them together into list of pairs
    - stops at the shorter list

---

<a id="syntax"></a>

# Section 3: Pattern Matching

- Top-to-bottom matching
    - catch-them-all case on the bottom
- Matching with lists
    - `(x:xs)`
    - `(x:y:xs)`
    - `[x]` is syntactic sugar for `(x:[])`
- *as patterns*
```haskell
-- captures the pattern
captial "" = "Empty string"
captial all@(x:xs) = "The first letter of " ++ all ++ "is" ++ [x]
```
---

# Guards

- Test if a property of value is true/false
- Drops to next guard until the condition is True
- `otherwise` is a catch-all
```haskell
functionName parameter
    | parameter < x = "..."
    | parameter < y = "..."
    | otherwise = ...
```

- Can *define* functions with backticks
```
a `myCompare` b
    | a > b = GT
    | a == b = EQ
    | otherwise = LT
```

---

# `where`

- bind names to values at the end of guards
- the name is then visible to all guards
```haskell
density mass volume
    | density < air ...
    | density ...
    where density = mass/ volume
          air = 1.2
-- all names must be on the same indent
```

- can pattern match
```haskell
initials firstname lastname = [f] ++ "." ++ [l] ++ "."
    where (f:_) = firstname
          (l:_) = lastname
```

- can define functions
```haskell
calcDensities xs = [density m v | (m, v) <- xs]  
    where density mass volume = mass / volume  
```

> IDIOM: make the helper functions in the `where` clause of a function

---

# `let`

- unlike `where`, it does not span across guards or the whole function
    - the names defined in `let` is only visible to the `in` expression
- unlike `where`, it can be use as *expressions* 
    ```haskell
    4 * (let a = 9 in a + 1) + 2
    [let square x = x*x in (square 5, square 3, square 2)]

    -- to bind names inline, use ;
    (let a = 100; b = 200; c = 300 in a*b*c, let foo="Hey "; bar = "there!" in foo ++ bar)  
    -- (6000000,"Hey there!")
    ```

- can be use in list comprehension, use in place of a `where` binding with a function
    - the bindings are visible in the output function and to predicates
    ```haskell
    calculateDensities xs = [ density | (m, v) <- xs, let density = m /v, density > 12]
    -- can't use the binding in (m,v) <- xs since it defined after
    ```

- if we defined the binding in the predicate, and it would only be visible to the predicate
```haskell
calculateDensitities xs = [ density | (m, v) <- xs, let density = m / v, let diff = (abs (m - v)) in diff > 1]
```
    
- If we omit the `in` part, then the binding becomes visible in the interactive session

---

# Case expressions

- are expressions!
- pattern matching on *function parameters* is syntactic sugar for case expressions
    ```haskell
    head' xs = case xs of [] -> error "No head for empty lists!"
                        (x:_) -> x
    ```

- can be used anywhere like an expression

---

<a id="hof"></a>

# Section 4: Higher Order Functions

- every function only takes **one** parameter
- all functions that accept multiple parameters are **curried** (reduced to functions taking one parameters)
```haskell
max 4 5
(max 4) 5
-- creates a function that takes a parameter and returns 4 or that parameter
-- then we pass in 5 to that function
```

- **function application**: `->` in the type definition, or putting a space
- function application is *right associative* in the type defintion
```haskell
max :: (Ord a) => a -> a -> a
-- same as
max :: (Ord a) => a -> (a -> a)
```

- curried functions are made up of **partially applied functions**
    - function that takes in as many parameters as we left out

- infix functions are *partially applied* using **sections**
- surround with parentheses and supply parameter on one side, it will create a function that takes another parameter to apply on the other side
    ```haskell
    divideByTen = (/10)
    -- dividebyTen 200 = 200 / 10

    isUpperAlphanum = (`elem` ['A'..'Z'])
    ```

> But sections don't work with - since `(-4)` means negative 4. So we have to use `(subtract 4)`

- cannot display partially applied functions since they are not part of the `Show` typeclass

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

---

## Maps and Filters

- `map` applies function to every element in the list
```haskell
map :: (a -> b) -> [a] -> [b]
map _ [] = []
map f (x:xs) = f x : map f xs
```

- `filter` keeps elements only if they satisfy a predicate
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
- syntax: `(\parameters -> function body)`
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
- `\x acc -> ...`

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

```haskell
sum (map sqrt [1..130])
-- vs
sum $ map sqrt [1..130]
```
- with spaces, function application is left associative
- with `$`, function application is right associative
- **lowest precedence** of any operator
    ```haskell
    -- (4+9) plus the sqrt of 3
    sqrt 3 + 4 + 9
    -- vs sqrt of 3 + 4 + 9
    sqrt $ 3 + 4 + 9

    -- save keystrokes
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
- right associative `(f (g (z x)))` = `f . g . z x`

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
    ```haskell
    compareWithHundred :: (Num a, Ord a) => a -> Ordering
    compareWithHundred x = compare 100 x
    compareWithHundred = compare 100

    -- instead of
    sum' :: (Num a) => [a] -> a  
    sum' xs = foldl (+) 0 xs 
    -- we can write it without the xs parameter
    sum' = foldl (+) 0

    -- function composition works well in free point style
    fn x = ceiling (negate (tan (cos (max 50 x))))
    fn = ceiling . negate . tan . cos . max 50
    ````

---

<a id="module"></a>

# Section 5: Modules

- **module**: group functions, types, typeclasses
- **program**: collection of modules loaded by the main module

- standard library split into *modules*
    - `Prelude` imported by default
    ```haskell
    -- all functions exported by Data.List are in the global namespace
    import Data.List

    -- weeds out duplicates from a list
    numUniques :: (Eq a) => [a] -> Int  
    numUniques = length . nub
    ```

- in GHCi, `:m + Data.List Data.Map Data.Set`

- can select functions to import
    ```haskell
    import Data.List (nub, sort)
    ```

- can select all functions but not certain ones
    - good for name conflicts between modules

    ```haskell
    import Data.List hiding (nub)
    ```

- **qualified imports** for name conflicts
    - requires that we call the entire module name before the function

    ```haskell
    -- filter and null conflicts with Prelude functions
    import qualified Data.Map
    -- can also do
    import qualified Data.Map as M

    x = Data.Map.filter ...
    y = M.filter ...
    ```

---

<a id="algebra"></a>

# Section 6: Algebraic data types

- for defining own data types
```hs
data Shape = Circle Float Float Float |
    Rectangle Float Float Float Float
```

- Circle, Rectangles are **value constructors**
```hs
-- value constructors are functions that return the datatype
-- note: Circle is not a type, it's a value constructor
:t Circle 
Circle :: Float -> Float -> Float -> Shape
Circle 10 20 10
```

- we can make the datatype part of a typeclass
```hs
-- Shape is part of the Show typeclass
data Shape = .... deriving (Show)
```

- nested value constructors when pattern matching
```hs
surface :: Shape -> Float
surface (Rectangle (Point x1 y1) (Point x2 y2)) = ....
```

- export data types in the module
    - if we don't export the value constructors, then the type can be made only with auxiliary functions and we cannot pattern match
```hs
module Shapes  
( Point(..) -- the .. exports all the value constructors  
, Shape(..) -- or we can specify the ones we want to export
, surface  
, nudge  
, baseCircle  
, baseRect  
) where 
```

---

## Record syntax

- alternative way of writing data types to avoid long list of fields
    ```hs
    data Person = Person { firstName :: String  
                        , lastName :: String  
                        , age :: Int  
                        , height :: Float  
                        , phoneNumber :: String  
                        , flavor :: String  
                        } deriving (Show)  

    ```
- advantages of the record syntax
    - automatically creates functions to lookup the field e.g. `flavor`, `lastName`
    - `show` displays it with the field name and brackets
    - don't have the put the fields in the proper order

--- 

## Type parameters

- **type constructors** takes type parameters to produce new types

    ```hs
    -- a is the type parameter, and can be any type
    -- it is the type that Maybe holds like `Maybe Int`, `Maybe Car`
    - `Maybe` is polymoprhic, can act like a `Maybe Int` or `Maybe Double` or `Nothing`
    data Maybe a = Nothing | Just a

    -- explicit type definition
    Just 10 :: Maybe Double
    Just 10.0
    ```

- before `=` is the type constructor
- after `=` is the value constructor


- can have record syntax with type constructors

    ```hs
    data Car a b c = Car { company :: a  
                        , model :: b  
                        , year :: c  
                        } deriving (Show)

    -- this would require that the a type parameter is of the Show typeclass
    tellCar :: (Show a) => Car String String a -> String  
    tellCar (Car {company = c, model = m, year = y}) = "This " ++ c ++ " " ++ m ++ " was made in " ++ show y
    ```

- don't put the type constraints into the data declarations
    - requires that the type declarations of the functions use them as well so would have to repeat them in the functions anyways
    - if we omit them, we can put the type declaration only on functions that uses them

    ```hs
    data (Ord k) => Map k v

    toList :: (Ord k) => ....
    ```

---

## Derived instances

- if a type can act like a certain typeclass, then we can make it an instance of the typeclass **by implementing the functions defined by the typeclass**
- derive the behavior of our types by using `deriving`

### Examples

- deriving { `Eq` }
    - match the value constructors, then match the fields in the value constructors
- `Show`
- `Read`
    - convert a string to values of the type
```hs
-- need explicit type annotation
ghci> read "Person {firstName =\"Michael\", lastName =\"Diamond\", age = 43}" :: Person  
Person {firstName = "Michael", lastName = "Diamond", age = 43} 
```
- `Ord`
    - the values that were defined first is considered smaller
- `Enum`
    - if all the value constructors are nullary (takes no fields)
    - for `succ` and `pred`
- `Bounded`
    - has a lowest and highest possible value
    ```hs
    data Day = Monday | Tuesday | Wednesday | Thursday | Friday | Saturday | Sunday  
               deriving (Eq, Ord, Show, Read, Bounded, Enum) 

    minBound :: Day
    >> Monday
    maxBound :: Day
    >> Sunday

    [minBound .. maxBound] :: [Day]  
    [Monday,Tuesday,Wednesday,Thursday,Friday,Saturday,Sunday]
    ```

---

## Type Synonyms


- e.g. `[Char]` is synonmous with `String`
```hs
type String = [Char]
```

- types can be parameterized (represented a type but is general)
```hs
type AssocList k v = [(k,v)]
-- type constructor is AssocList
-- concrete type is AssocList Int String
```

- can have partially applied type parameters to get type constructors
```hs
type IntMap v = Map Int v
-- same thing as 
type IntMap = Map Int
```

- `data Either a b = Left a | Right b ...`
    - can be either value of Left type a or value of Right type b
    - good for when errors are Left value, results are Right value
