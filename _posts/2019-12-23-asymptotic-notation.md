---
layout: post
title: "Asymptotic Notation"
author: "DAEUN"
---

We can use asymtotic notation to show running times of algorithms. There are $\O$(), $\Omega$(), $\Theta$(), $\o$(), and $\omega$() to describe running times.

## $\O$ - notation (Big O)
When the highest degree of any term of function $\f(n)$ is **lower or equal** to the degree of $\g(n)$, we can say that $\f(n) \in \O(g(n))$

For instance, $\O(n^{2}) = \{n^{2}, n, 1\}$

* We mostly will use this notation to describe the running times of algorithms, since it indicates \$O(f(n))$ of worst case running time and it must be below \$O(f(n))$ time in other cases.

## $\o$ - notation (o)
This is similar to the definition of $\O$ - notation, except for the case when the highest degree of function $\f(n)$ is equal to the degree of $\g(n)$

## $\Omega$ - notation (Big Omega)
When the highest degree of any term of function $\f(n)$ is **higher or equal** to the degree of $\g(n)$, we can say that $\f(n) \in \Omega(g(n))$

For instance, $\Omega(n^{2}) = \{n^{2}, n^{3}, n^{4}, \dots\}$

## $\omega$ - notation (omega)
This is similar to the definition of $\Omega$ - notation, except for the case when the highest degree of function $\f(n)$ is equal to the degree of $\g(n)$

## $\Theta$ - notation (Big Theta)
When the highest degree of any term of function $\f(n)$ is **equal** to the degree of $\g(n)$, we can say that $\f(n) = \Theta(g(n))$

For instance, $\Theta(n^{2}) = \{2n^{2}, 3n^{2}, 4n^{2}, \dots\}$

* $\Theta(f(n)) = \O(f(n)) \cap \Omega(f(n))$ is true.
* $\Theta(1)$ indicate constant or constant function.