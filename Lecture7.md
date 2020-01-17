# Programming with Categories
## Lecture 7

`Pair a b == (a, b)`

## Universal Properties
One-stop shop for:
1. *Products* `a x b`: maps into `a` and `b`
2. *Coproducts* `a + b`: maps out of `a` and `b`
3. *Exponentials* `b^a`: maps with knob `a` into `b`

### Coproducts in `Set`
The *coproduct* in `Set` is given by the *disjoint (tagged)* union.

Can be tagged with whatever we want, `0` and `1`, `Left` and `Right`, etc.

Def: Let `C` be a category, and `c,d \in Ob(C)`. A *coproduct* of `c` and `d` consists of:
1. an object `c + d \in Ob(C)`
2. a morphism `Left: c -> c + d`
3. a morphism `Right: d -> c + d`

satisfying the property that for any `X \in Ob(C)` and any morphisms `L: c -> X` and `R: d -> X` there is a unique morphism `either L R: c + d -> X` s.t. the diagram commutes
```
              L
      c ---------------
 Left |               |
      v   either L R  v
    c + d ----------> X
Right |               ^
      v       R       |
      d ---------------
```

In Haskell, disjoint unions are tagged with `Left` and `Right`.
```haskell
  data Either a b = Left a | Right a
  -- Left :: a -> Either a b
  -- Right :: b -> Either a b
  either :: (a -> c) -> (b -> c) -> Either a b -> c
  either f g Left a  = f a
  either f g Right b = g b
```

`bifunctor :: Hask x Hask -> Hask`
```haskell
  bimap :: (a -> a') -> (b -> b') -> Either a b -> Either a' b'
  bimap f g Left a  = Left f a b'
  bimap f g Right b = Right a' g b
```

### Exponentials
In `Set`, `Setting x Input -> Output === Setting -> Input -> Output`.

In Haskell, `b^a == a -> b`.
```
  curry :: ((a, b) -> c) -> a -> b -> c
  curry f a b = f (a, b)

  uncurry :: (a -> b -> c) -> (a, b) -> c
  uncurry f (a, b) = f a b
```
Diagram commutes
```
                 curry f
              a ---------> b -> c

                      f
            a x b ---------> c
              |              ^
(curry f, id) |              |
              v        eval  |
         (b -> c, b) ---------
```
