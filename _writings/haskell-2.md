---
base-title: Haskell notes (Syntax in Functions)
base-description: pattern matching, `where`, `let`, `case`
date: Oct 26, 2025
---

# Pattern Matching

- Top-to-bottom matching
- catch-them-all case on the bottom
- Matching with lists
    - `(x:xs)` or `(x:y:xs)`
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

- **Pattern guards**: check if result of function satisfies the pattern

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
- unlike `where`, can be use as *expressions* 

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
