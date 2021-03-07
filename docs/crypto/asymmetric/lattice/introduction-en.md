[RU](./introduction.md) | [EN](./introduction-en.md)

# Basic Introduction

## Lattice Definition

The lattice is the linear combination of all integer coefficients of n (<img src="https://render.githubusercontent.com/render/math?math=m\geq n">) linearly independent vectors <img src="https://render.githubusercontent.com/render/math?math=b_i(1\leq i \leq n)"> of the m-dimensional Euclidean space <img src="https://render.githubusercontent.com/render/math?math=R^m">, ie <img src="https://render.githubusercontent.com/render/math?math=L(B)=\{\sum_{i=1}^{n}x_ib_i:x_i \in Z, 1 \leq i \leq n\}">

Here B is a collection of n vectors, we call

- These `n` vectors a set of bases of the lattice `L`.
- The rank of the lattice `L` is `n`.
- The number of bits in `L` is `m`.

If `m = n`, then we call this format full rank.

Of course, the space can be other groups instead of <img src="https://render.githubusercontent.com/render/math?math=R^m">.

## Basic Definition in Lattices

### Successive Minimum

Let lattice `L` be a lattice in the m-dimensional Euclidean space <img src="https://render.githubusercontent.com/render/math?math=R^m"> with rank `n`, then the continuous minimum length of `L` (successive minima) is <img src="https://render.githubusercontent.com/render/math?math=\lambda_1,\ldots,\lambda_n \in R">, where for any <img src="https://render.githubusercontent.com/render/math?math=1 \leq i\leq n, \lambda_i"> is the minimum value to satisfy that for `i` linearly independent vectors <img src="https://render.githubusercontent.com/render/math?math=v_i, ||v_j||\leq \lambda_i,1\leq j\leq i">.

Obviously we have <img src="https://render.githubusercontent.com/render/math?math=\lambda_i \leq \lambda_j ,\forall i < j">.

## Calculating Difficult Problems in the Lattice

**Shortest Vector Problem (SVP)**: Given the lattice `L` and its base vector `B`, find the non-zero vector `v` in the lattice `L` such that for any other non-zero vector `u` in the lattice, <img src="https://render.githubusercontent.com/render/math?math=||v|| \leq ||u||">.

**<img src="https://render.githubusercontent.com/render/math?math=\gamma">-Approximate Shortest Vector Problem (SVP-<img src="https://render.githubusercontent.com/render/math?math=\gamma">)**: Given a fixed `L`, find the non-zero vector `v` in the lattice `L` such that for any other non-zero vector `u` in the lattice, <img src="https://render.githubusercontent.com/render/math?math=||v|| \leq \gamma||u||">.

**Successive Minima Problem (SMP)**: Given a lattice `L` of rank `n`, find `n` linearly independent vectors <img src="https://render.githubusercontent.com/render/math?math=s_i"> in lattice `L`, satisfying <img src="https://render.githubusercontent.com/render/math?math=\lambda_i(L)=||s_i||, 1 \leq i \leq n">.

**Shortest Independent Vector Problem (SIVP)**: Given a lattice `L` of rank `n`, find `n` linear independent vectors <img src="https://render.githubusercontent.com/render/math?math=s_i"> in lattice `L`, satisfying <img src="https://render.githubusercontent.com/render/math?math=||s_i|| \leq \lambda_n(L), 1 \leq i \leq n">.

**Unique Shortest Vector Problem (uSVP-<img src="https://render.githubusercontent.com/render/math?math=\gamma">)**: Given a fixed `L`, satisfying <img src="https://render.githubusercontent.com/render/math?math=\lambda_2(L) > \gamma \lambda_1(L)">, find the shortest vector of the cell.

**Closest Vector Problem (CVP)**: Given the lattice `L` and the target vector <img src="https://render.githubusercontent.com/render/math?math=t \in R^m">, find a non-zero vector `v` in a lattice such that for any non-zero vector `u` in the lattice , satisfy <img src="https://render.githubusercontent.com/render/math?math=||vt|| \leq ||ut||">.