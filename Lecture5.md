# Programming with Categories
## Lecture 5

### Functors in Haskell
```Haskell
  data Identity a = MkId a
```
- `Identity` is called a *type constructor*
- `MkId` is called a *data constructor*

means that `MkId :: a -> Identity a`, i.e. `MkId` is a mapping of objects.

To make `Identity` a functor, we must implement a mapping of morphisms, i.e. `mapId :: (a -> b) -> Identity a -> Identity b` (curried). E.g. for `f: a -> b`, we can make
```
  mapId f MkId a = MkId fa
```

### Type Classes
*Type classes* allow you to make type constructors into variables.
```Haskell
  typeclass Functor f where
  fmap f :: (a -> b) -> fa -> fb
```
To tell the compiler that you are provided an implementation of a type class use keyword `instance`.
```Haskell
  instance Functor Identity where
  fmap g MkId a = MkId ga
```

### Algebraic Data Types
```Haskell
  data Maybe a = Nothing | Just a
  instance Functor Maybe where
  fmap f Nothing = Nothing
  fmap f Just a  = Just fa
```
