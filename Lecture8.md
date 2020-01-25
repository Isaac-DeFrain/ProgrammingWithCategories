# Programming with Categories
## Lecture 8

Functors as models of the source category.

To talk about relationships between models, we need morphisms in the target category.

A *natural transformation* is a family of morphism between models.

Let `F,G: C -> D` be functors (models of `C` in `D`). A natural transformation `n: F => G` is a relationship between the models (diagram commutes)
```
f: a -> b in C

         Ff
    Fa ------> Fb
     |         |
n(a) |         | n(b)
     v   Gf    v
    Ga ------> Gb
```

### In Haskell
**A natural transformation is a polymorphic function in Haskell.**

```haskell
  type Nat f g = forall a. f a -> g a
```

Endofunctor = container - "keep it fresh"

Polymorphic functions in Haskell are "natural" because of parametric polymorphism (only one formula for all types).

```haskell
      head :: [a] -> a       (unsafe, returns error if list is empty)
  safeHead :: [a] -> Maybe a (safe, returns Nothing if list is empty)
```

Functor category: objects are functors, morphism are natural transformations

### Adjunction
```
    f(a,b)     currying     f a b
  (a x b -> c) --------> (a -> b -> c)
               <--------
               uncurrying
```

Categorical notation

```
  C(a x b, c) ----> C(a, c^b)
              <----
```

Def: An *adjunction* is
```
      L
  C <--- A
    --->
      R
```
with for every `c \in Ob(C), a \in Ob(A)`, there exists an isomorphism
```
            i(a,c)
  C(La, c) -------> A(a, Rc)
```
which is natural in `a` and `c`. Natural in `a` and `c` means the following diagrams commute
```
  f: c -> c' in C               |  g: a -> a' in A
                                |
            i(a,c)              |             i(a,c)
  C(La, c) -------> A(a, Rc)    |   C(La, c) -------> A(a, Rc)
     |                 |        |      |                 |
 -;f |                 | -;Rf   | Lg;- |                 | g;-
     v      i(a,c')    v        |      v      i(a',c)    v
  C(La, c') ------> A(a, Rc')   |   C(La', c) ------> A(a', Rc)
```

**Finish notes about adjunctions
