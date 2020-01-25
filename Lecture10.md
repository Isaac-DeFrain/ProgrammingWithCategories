# Programming with Categories
## Lecture 10

Coalgebra is an algebra with arrows turned around, i.e. `X -> F(X)`

Final = Terminal

### Examples of Initial Algebras
Find the initial algebra and final coalgebra following `F: Set -> Set`
1. `F(S) := Nat` constant (all objects mapped to `Nat`, all morphisms to `id(Nat)`)

```
f: F(S) ---> S // any F-algebra
              id
        Nat ------> Nat
         |           |
      id |           | f
         v     f     v
  Nat = F(S) ------> S
```

2. `F(S) := S` identity

```
f: F(S) ---> S // any F-algebra
         !
 { } --------> { }
  |             |
! |             | !
  v     f       v
  X ----------> X
```

3. `F(S) = 1 + S`

```
f: F(S) ---> S // any F-algebra
                   <0, succ>
           1 + Nat ---------> Nat
             |                 |
<id,cata(f)> |                 | cata(f)
             v      <x*,f>     v
           1 + X ------------> X

cata(f)(0) = x*
cata(f)(1) = f(x*)
cata(f)(2) = f(f(x*))
...
```

4. `F(S) := 1 + Nat x S`

```
  List(Nat)
```

5. `F(S) := Nat + Nat x S x S`

```
  Binary trees with Nat-labeled nodes
```


### Final Coalgebras
**revisit**

#### Lambek's Theorem
Let `a: F(\mu_F) -> \mu_F` be an initial algebra (i.e. `\mu_F` is the carrier and `a` is the structure map). Then `a` is an isomorphism.

*Proof*: Suppose `a: F(\mu_F) -> \mu_F` is initial. Then `F(a): FF(\mu_F) -> F(\mu_F)` is an algebra. The following diagrams commute. Since this algebra is initial, there is a unique morphism `\mu_f -> \mu_F` which has to be the identity.
```
                 a
    F(\mu_F) ---------> \mu_F          a . cata = id_\mu_F
        |                 |            F(a) . F(cata) = cata . a = id_F(\mu_F) = id_\mu_F
F(cata) |                 | cata
        v       Fa        v
   FF(\mu_F) -------> F(\mu_F)
        |                 |
     Fa |                 | a
        v        a        v
    F(\mu_F) ---------> \mu_F
```

### Directed Colimits
(In Set)

Idea: "telescoping union"

Suppose we have an infinite sequence of sets
```
    f01      f12      f23  ...
  A0 ----> A1 ----> A2 ----> ...
```

Its *colimit* is the set
```
  {(n, a) : n \in Nat, a \in An } / ~
```

where `~` is the equivalence relation given by `(m, a) ~ (n, b)` if `m <= n` and `fmn(a) = b` where `fmn = f(n-1)n . ... . fm(m+1)`.

#### Adamek's Theorem
Suppose `F: Set -> Set` is an endofunctor. If a colimit of the sequence
```
          !             F.!                F.F.!
  \empty --> F(\empty) ----> F(F(\empty)) ------> ...
```

exists and `F` preserves it, then it is an initial algebra (all initial algebras are isomorphic).

Example: `F(S) = 1 + Nat x S`
```
          !
  \empty --> 1 + Nat x \empty
                 ||
                 1 ----------> 1 + Nat x 1
                                   ||
                                 1 + Nat ---> 1 + Nat x (1 + Nat)
                                                      ||
                                               1 + Nat + Nat^2 ----> ...
```
The colimit of this sequence is `List(Nat)`.
