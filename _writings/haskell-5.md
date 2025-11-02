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
- `foldl'`
- `foldl1'`
- `concat`
- `concatMap`
- `and`
- `or`
- `any`
- `all`
- `iterate`
- `splitAt`
- `takeWhile`
- `dropWhile`
- `span`
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


