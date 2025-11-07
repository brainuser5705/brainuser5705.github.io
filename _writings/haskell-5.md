---
base-title: Haskell (Functions in Modules)
base-description: functions in `Data.List`, `Data.Char`, `Map`, `Set` 

time: Nov 2, 2025
---

# Data.List

- no need for qualified imports

- `intersperse`

```haskell
intersperse '.' "monkey"
> "m.o.n.k.e.y"
```

- `intercalate`
    - insert element between the list and flattens

```haskell
intercalate " " ["hello", "there", "world"]
> "hello there world"
```

- `transpose`
    - convert collect to rows, and vice versa

- `foldl'`, `foldl1'`
    - regular folds are lazy and will not calculate the intermediate accumulator value
    - instead it will make a promise and put it on the stack as a **thunk** and the stack can overflow
    - these stricter folds will calculate it right away

- `concat`
    - flattens the list of lists

```haskell
concat [[1,2,3], [4,5,6], [7,8,9]]
> [1,2,3,4,5,6,7,8,9]
```

- `concatMap`
    - map function to list and concatenating results

- `and`
    - takes list of boolean values and returns True only if all elems are True

```haskell
and $ map (>4) [5,6,7,8]
> True
```

- `or`
    - at least one True

- `any`, `all`
    - takes predicate and check if any or 
    - same as mapping over list and doing `and`, `or`

- `iterate`
    - keeping applying function to value and the results into an infinite list

```haskel
take 10 $ iterate (*2) 1
```
- `splitAt`
    - split list at # of elements and returns the two parts in two lists

```haskell
splitAt 3 "heyman"
> ("hey", "man")
```

- `takeWhile`
- `dropWhile`
    - like `takeWhile` but drops the elements until predicate is `False` and return the rest of the list

- `span`
    - like `takeWhile` but returns the two halves in a tuple

- `break`

- `sort`
- `group`
- `inits`
- `tails`
- `isInfixOf`
- `isPrefixOf`
- `isSuffixOf`
- `elem`
- `notElem`
- `partition`
- `find`
- `elemIndex`
- `elemIndices`
- `findIndex`
- `findIndices`
- `zip3, zip4, zipWith3, zipWith4`
- `lines`
- `unlines`
- `words`
- `unwords`
- `nub`
- `delete`
- `\\`
- `union`
- `intersect`
- `insert`
- `genericLength`
- `genericTake`
- `genericDrop`
- `genericSplitAt`
- `genericIndex`
- `genericReplicate`
- `nubBy`
- `deleteBy`
- `unionBy`
- `intersectBy`
- `groupBy`
- `sortBy`
- `insertBy`
- `maximumBy`
- `minimumBy`
- 
## Data.Char

`isControl`
`isSpace`
`isLower`
`isUpper`
`isAlpha`
`isAlphaNum`
`isPrint`
`isDigit`
`isOctDigit`
`isHexDigit`
`isLetter`
`isMark`
`isNumber`
`isPunctuation`
`isSymbol`
`isSeparator`
`isAscii`
`isLatin1`
`isAsciiUpper`
`isAsciiLower`
`
`toUpper`
`toLower`
`toTitle`
`digitToInt`
`intToDigit`
`ord`

## Data.Map

`fromList`
`empty`
`insert`
`null`
`size`
`singleton`
`lookup`
`member`
`map`
`filter`
`toList`
`keys`
`elems`
`fromListWith`
`insertWith`

## Data.Set

`fromList`
`intersection`
`difference`
`union`


