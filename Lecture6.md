# Programming with Categories
## Lecture 6

Quasi-category *Hask*: objects are types, morphisms are functions

### Terminal Objects
Def: A *terminal object* in `C` is an object `T \in Ob(C)` s.t. for every `c \in Ob(C)`, there exists a unique morphism `c -> T`.

Terminal objects in `Set` are singleton sets.

In `Hask`:
```Haskell
  data Terminal = MkTerm
  bang :: a -> Terminal  -- type signature
  bang x = MkTerm
```
acts like a terminal object.

Unit type:
```Haskell
  data () = ()
  () :: ()
```

### Initial Objects
Def: An *initial object* in `C` is an object `I \in Ob(C)` s.t. for every `c \in Ob(C)`, there exists a unique morphism `I -> c`.

Initial object in `Set` is the empty set.

In `Hask`:
```Haskell
  data Void
  absurd :: Void -> a
  absurd x = absurd x  -- type checks so compiler is happy :)
```

### Products
Let `A, B, c \in Ob(Set)`. The product of the functions `c -> A` and `c -> B` is the same as a function `c -> A x B` and vice versa.
```
  Set(c, A) x Set(c, B) ~ Set(c, A x B)
```

Universal definition of product. (mapping in property)

### Haskell
Products in Haskell
```Haskell
  data Pair a b = MkPair a b
  fst :: Pair a b -> a
  fst MkPair a b = a
  snd :: Pair a b -> b
  snd MkPair a b = b

  tuple :: (c -> a, c -> b) -> c -> (a, b)
  tuple f g c = \c -> MkPair fc gc

  untuple :: (c -> Pair a b) -> (c -> a, c -> b)
  untuple h = (\c -> fst hc, \c -> snd hc)  -- alternatively: untuple h = (fst.h, snd.h)
```
Since `Pair` has two type variables, we must be able to lift two functions at the same time.

Say, `f :: a -> a'` and `g :: b -> b'`. Need to lift these to `Pair a b -> Pair a' b'`.
```Haskell
  bimap :: (a -> a', b -> b') -> (Pair a b -> Pair a' b')
  bimap (f, g) = tuple (f.fst) (g.snd)
```

Exercises: Draw diagrammatically, then translate to code.
1. `(a, b) -> (b, a)`
2. `a -> ((), a)`
3. `a -> (a, ())`
4. `(a, (b, c)) -> ((a, b), c)`
