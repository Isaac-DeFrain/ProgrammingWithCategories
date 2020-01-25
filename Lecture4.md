# Programming with Categories
## Lecture 4

Let `*2 = (a) -> (b)`.

Q: How many functors: `Set -> *2`

A: 3
  1. All sets mapped to `(a)`
  2. All sets mapped to `(b)`
  3. Empty set mapped to `(a)`, nonempty sets mapped to `(b)`

In Haskell, to declare a functor:
1. Declare a type constructor `F` with one type variable.
2. Declare that `F` is an instance of `Functor` class, i.e. say what `fmap` means.

Example:
```Haskell
data WithString a = WithStr (a, String) -- a x String
instance Functor WithString where
  fmap f WithStr (x, s) = WithStr (fx, s)
```
Here, `WithString` is the type name and `WithStr` is the constructor (convention is that these have the same name). In `fmap`, variable `x` is of type `a` (convention is that `a` is also used for the variable name).

### Natural transformations
Def: A natural transformation `n` between functors `F,G: C -> D`, written `n: F => G`, satisfies
1. for each `c \in Ob(C)`, there is a morphism `n(c): Fc -> Gc` s.t.
  - for each morphism `f: c1 -> c2`, the following diagram commutes

```
       F(f)
Fc1 ----------> Fc2
 |               |
 | n(c1)         | n(c2)
 |               |
 v     G(f)      v
Gc1 ----------> Gc2
```

Natural transformations between functors `*1 -> C` are isomorphic to the morphisms in `C`.
