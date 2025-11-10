---
base-title: Haskell notes (Own Types, Typeclasses)
base-description:

time: November 7, 2025
---

# Algebraic data types

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

# Record syntax

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

# Type parameters

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

# Derived instances

- if a type can act like a certain typeclass, then we can make it an instance of the typeclass **by implementing the functions defined by the typeclass**
- derive the behavior of our types by using `deriving`

## Examples

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

# Type Synonyms


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
