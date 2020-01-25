# Programming with Categories
## Lecture 9

### Recursive Data Types
Fully recursive data type for algebraic expressions.

```haskell
  data Ex = Plus Ex Ex
          | Times Ex Ex
          | Const Double
          | Var String
```
#### Building Recursive Terms
Non-recursive data type for building algebraic expressions. `ExF` is a functor.
```haskell
  data ExF a = Plus a a
             | Times a a
             | Const Double
             | Var String
```

For `a = Void`, we can construct constants and variables, but no sums or products.
```haskell
  data ExF Void = Const Double | Var String
  type Ex1 = ExF Void
```

Now letting `a = ExF Void`, we can construct sums and products of variables and constants.
```haskell
  data ExF Ex1 = Plus Ex1 Ex1
               | Times Ex1 Ex1
               | Const Double
               | Var String
  type Ex2 = ExF Ex1
```

Etc.

#### Evaluation of Recursive Terms
**Revisit**

### `F`-Algebras
IDEA: Given an endofunctor, you get a type.

Def: Given a functor `F: C -> C`, an *`F`-algebra* `(X, s)` is:
  - an object `X \in Ob(C)` // *carrier*
  - a morphism `s: FX -> X` // *structure map*

Think: `F` constructs terms and `s` evaluates terms.

Def: A *morphism of `F`-algebras* `(X, s) -> (Y, t)` is a morphism `f: X -> Y` in `C` s.t. the following diagram commutes.
```
          s
   FX ---------> X
    |            |
 Ff |            | f
    v     t      v
   FY ---------> Y
```

Fact: `F`-algebras and their morphisms form a category! (composition?, identity?, laws?)

#### In Haskell
```haskell
  data F a = Nil | Cons Int a
  deriving Functor
```

`fmap` that will be derived for `F`:
```
  F: C -> C, X |-> 1 + Int x X
  f: X -> Y, Ff = Id_1 + Id_Int x f
```

#### Initial Algebras
Initial object in the category of `F`-algebras and `F`-morphisms.

Def: Given a functor `F`, and *initial algebra* of `F` is a morphism `in: FA -> A` s.t. for all morphisms `m: FX -> X`, there exists a unique morphism `f: A -> X` s.t. the following diagram commutes.
```
        in
   FA ------> A
    |         |
 Ff |         | f
    V   m     v
   FX ------> X
```

The initial algebra for the above Haskell functor `F` has carrier `ListInt`.

```
  F ListInt = 1 + Int x ListInt ---> ListInt // structure map
                   * |---> []
              (n, x) |---> [n,...x]
```

Another `F`-algebra (carrier `Int` and structure map `s: F Int -> Int`):
```
                         s
  F Int = 1 + Int x Int ---> Int
               * |---> 0
          (m, n) |---> m + n
```

Catamorphism: `cata(s): ListInt -> Int` s.t. the following diagram commutes.
```
                  in
      F ListInt ------> ListInt
          |                |
F cata(s) |                | cata(s)
          v       s        v
        F Int ----------> Int
```
