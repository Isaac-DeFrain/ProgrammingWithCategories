# Programming with Categories
## Lecture 11

A colimit differs from a coproduct by identifying some elements.

Lambek's lemma: If `a: FX -> X` is an initial `F`-algebra, then `b: X -> FX` exists, i.e. the structure map is an isomorphism (`a` and `b` are inverses).

### Bringing the previous ideas into Haskell speak
#### Fixed Points
Idea: Fixed points in Haskell
```
  data Fix f = MkFix(f(Fix f))
  MkFix :: f(Fix f) -> Fix f
```

Haskell definition of `Fix`
```
  newtype Fix f = Fix(f(Fix f))
    Fix :: f(Fix f) -> Fix f

  // newtype automatically generates this inverse (from Lambek's lemma)
  unFix :: Fix f -> f(Fix f)
  unFix(Fix x) = x
```

The semantics of `newtype` is exactly like `data`. It can only be used when there is one data constructor and this data constructor takes only one argument.

#### Algebras
```
  type Algebra f a = fa -> a

        f - functor
        a - carrier
  fa -> a - structure map (evaluator)
```

Algebra for the carrier `Fix f`
```
  Algebra f (Fix f) = f(Fix f) -> Fix f
```

The data constructor for `Fix` is an algebra.

What does it mean for this algebra to be initial? The following (inner) diagram commutes and for any algebra `alg: fa -> a`, `cata alg: Fix f -> a` is unique!
```
             fmap(cata alg)
    f(Fix f) --------------> fa        cata alg = alg . fmap(cata alg) . unFix
       ^ |                   |         cata :: Algbra f a -> Fix f -> a
 unFix | | Fix               | alg
       | v      cata alg     v
     Fix f ----------------> a
```

Revisiting expressions.
```
  data ExprF a = PlusF a a
               | TimesF a a
               | Const Double
               | Var String

  // Evaluation is an algebra (structure map with carrier)
  eval :: ExprF Double -> Double
          Algebra ExprF Double
```

Defining the "fully" recursive data type `Ex` (fixed point)
```
  type Ex = Fix ExprF
```

"smart constructors"
```
  // leaves
  num :: Double -> Ex
  num a = Fix(Const a)

  var :: String -> Ex
  var a = Fix(Var a)

  // nodes
  add :: Ex -> Ex -> Ex
  add a b = Fix(PlusF a b)

  mult :: Ex -> Ex -> Ex
  mult a b = Fix(TimesF a b)
```

##### Lists
`List` is a recursive data structure.
```
  data List a = Nil | Cons a (List a)
```

What functor is behind it?
```
  data ListF a b = NilF | ConsF a b
```

i.e. `ListF(a, b) = 1 + a x b` and
```
  type List a = Fix(ListF a)
```

`fold` is the catamorphism for `List`

`ListF`-algebra
```
  alg :: ListF a b -> b
  alg NilF = b
  alg (Cons a b) :: a -> b -> b
```
