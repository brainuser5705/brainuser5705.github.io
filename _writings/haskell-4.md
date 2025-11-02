---
base-title: Haskell notes (Modules)
base-description:
time: November 1, 2025
---

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

- can select all functions but certain ones
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

# Data.List

intersperse
intercalate
transpose
foldl'
foldl1'
concat
concatMap
and
or
any
all
iterate
splitAt
takeWhile
dropWhile
span
break
sort
group
inits
tails
isInfixOf
isPrefixOf
isSuffixOf
elem
notElem
partition
find
elemIndex
elemIndices
findIndex
findIndices
zip3, zip4, zipWith3, zipWith4
lines
unlines
words
unwords
nub
delete
\\
union
intersect
insert
genericLength
genericTake
genericDrop
genericSplitAt
genericIndex
genericReplicate
nubBy
deleteBy
unionBy
intersectBy
groupBy
sortBy
insertBy
maximumBy
minimumBy

## Data.Char

isControl
isSpace
isLower
isUpper
isAlpha
isAlphaNum
isPrint
isDigit
isOctDigit
isHexDigit
isLetter
isMark
isNumber
isPunctuation
isSymbol
isSeparator
isAscii
isLatin1
isAsciiUpper
isAsciiLower

toUpper
toLower
toTitle
digitToInt
intToDigit
ord

## Data.Map

fromList
empty
insert
null
size
singleton
lookup
member
map
filter
toList
keys
elems
fromListWith
insertWith

## Data.Set

fromList
intersection
difference
union



