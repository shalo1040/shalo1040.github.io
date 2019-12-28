---
layout: post
title: "Asymptotic Notation"
author: "DAEUN"
---

We can use asymtotic notation to show running times of algorithms. There are O(), &Omega;(), &Theta;(), o(), and &omega;() to describe running times.

## O - notation (Big O)
When the highest degree of any term of function _f(n)_ is **lower or equal** to the degree of _g(n)_, we can say that _f(n)_ &isin; O(_g(n)_)

For instance, O(n<sup>2</sup>) = {n<sup>2</sup>, n, 1}

* We mostly will use this notation to describe the running times of algorithms, since it indicates O(_f(n)_) of worst case running time and it must be below O(_f(n)_) time in other cases.

## o - notation (o)
This is similar to the definition of O - notation, except for the case when the highest degree of function _f(n)_ is equal to the degree of _g(n)_

## &Omega; - notation (Big Omega)
When the highest degree of any term of function _f(n)_ is **higher or equal** to the degree of _g(n)_, we can say that _f(n)_ &isin; &Omega;(_g(n)_)

For instance, &Omega;(n<sup>2</sup>) = {n<sup>2</sup>, n<sup>3</sup>, n<sup>4</sup>}

## &omega; - notation (omega)
This is similar to the definition of &Omega; - notation, except for the case when the highest degree of function _f(n)_ is equal to the degree of _g(n)_

## &Theta; - notation (Big Theta)
When the highest degree of any term of function _f(n)_ is **equal** to the degree of _g(n)_, we can say that _f(n) =_ &Theta;(_g(n)_)

For instance, &Theta;(n<sup>2</sup>) = {2n<sup>2</sup>, 3n<sup>2</sup>, 4n<sup>2</sup>}

* &Theta;(_f(n)_) = O(_f(n)_) &isin; &Omega;(_f(n)_) is true.
* &Theta;(1) indicate constant or constant function.
<br><br><br>
reference: T.H. Cormen, C.E. Leiserson, R.L. Rivest, and C. Stein, _Introduction to Algorithms_, 3rd ed. MIT Press, 2009.