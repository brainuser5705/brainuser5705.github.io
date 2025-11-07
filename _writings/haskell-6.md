---
base-title: Haskell notes (Own Types, Typeclasses)
base-description:

time: November 7, 2025
---

# Algebraic data types

- define own data type
```hs
data Shape = Circle Float Float Float |
    Rectangle Float Float Float Float
```
- Circle, Rectangles are the **value constructors**
    - they are functions, so they return the value of a data type
    - we can pattern match against value constructors
    - can bind fields to the constructor name

```hs
:t Circle 
Circle :: Float -> Float -> Float -> Shape
Circle 10 20 10

-- Circle is not a type, it's only a value constructor
```

- to make the `Shape` type part of the `Show` typeclass
```hs
data Shape = .... deriving (Show)
```

- can have nested value constructors when pattern matching
```hs
surface :: Shape -> Float
surface (Rectangle (Point x1 y1) (Point x2 y2)) = ....
```

- export data types in the module
```hs
module Shapes  
( Point(..) -- .. would export all the value constructors  
, Shape(..) -- or we can specify the ones we want to export
, surface  
, nudge  
, baseCircle  
, baseRect  
) where 
```
- if we don't export the value constructors then it can only be made with auxiliary functions (more abstract)
- cannot pattern match against value constructors

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
    - creates functions to lookup the field e.g. `flavor`, `lastName`
    - `show` displays it with the field name and brackets
    - don't have the put the fields in the proper order

# Type parameters

- **type constructors** takes types as parameters to produce new types

```hs
data Maybe a = Nothing | Just a
-- a is the type parameter

Just 10 :: Maybe Double
Just 10.0
```
- it is the type that Maybe holds like `Maybe Int`, `Maybe Car`
- compare to value constructors that already define a type
- `Maybe` is polymoprhic, can act like a `Maybe Int` or `Maybe Double` or `Nothing`

- can have record syntax with type constructors
```hs
data Car a b c = Car { company :: a  
                     , model :: b  
                     , year :: c  
                     } deriving (Show)

tellCar :: (Show a) => Car String String a -> String  
tellCar (Car {company = c, model = m, year = y}) = "This " ++ c ++ " " ++ m ++ " was made in " ++ show y
-- this would require that the c type parameter is of the Show typeclass
```

- don't put the type constraints into the data declarations
    - requires that the type declarations of the functions use them as well
    - would have to repeat them in the functions anyways
    - if we omit them, we can put the type declaration only on functions that uses them
```hs
data (Ord k) => Map k v

toList :: (Ord k) => ....
```

 - before `=` is the type constructor
 - after `=` is the value constructor

# Derived instances

- if a type can act like a certain typeclass, then we can make it an instance of the typeclass **by implementing the functions defined by the typeclass**
- derive the behavior of our types by using `deriving`


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

# Type Synonyms

- `type`
- e.g. `[Char]` is synonmous with `String`

```hs
type String = [Char]
```

- types can be parameterized, represented a type but is general
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
    - errors are Left value, results are Right value


