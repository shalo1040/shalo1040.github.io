---
layout: post
title: "Methods for Solving Recurrencs"
author: "DAEUN"
---

There are several methods for solving recurrences: repetition, substitution, and master method. 

### Repetition Method(반복법)
This is calculating T(n) repeatably. T(n) = 8\*T(n/2) + &Theta;(n<sup>2</sup>) (n > 1), for instance, can be solved with this method as following:
<br>
T(n) = 8\*T(n/2) + &Theta;(n<sup>2</sup>)<br>
&nbsp; &nbsp; &nbsp; &nbsp; = 8<sup>2</sup>\*T(n/2<sup>2</sup>) + &Theta;(n<sup>2</sup>/2<sup>2</sup>) + &Theta;(n<sup>2</sup>/2<sup>0</sup>)<br>
&nbsp; &nbsp; &nbsp; &nbsp; = 8<sup>3</sup>\*T(n/2<sup>3</sup>) + &Theta;(n<sup>2</sup>/2<sup>4</sup>) + &Theta;(n<sup>2</sup>/2<sup>2</sup>) + &Theta;(n<sup>2</sup>/2<sup>0</sup>)<br>
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ...<br>
&nbsp; &nbsp; &nbsp; &nbsp; = 8<sup>k</sup>\*T(n/2<sup>k</sup>) + &Theta;(n<sup>2</sup>/2<sup>2(k-1)</sup>) + ... + &Theta;(n<sup>2</sup>/2<sup>2</sup>) + &Theta;(n<sup>2</sup>/2<sup>0</sup>)<br>
&nbsp; &nbsp; &nbsp; &nbsp; = 8<sup>lgn</sup>\*T(1) + &Theta;(n<sup>2</sup>\*(4n/3 - 8/3n)) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; (because n/2<sup>k</sup> = 1)<br>
&nbsp; &nbsp; &nbsp; &nbsp; = 8<sup>lgn</sup>\*&Theta;(1) + &Theta;(n<sup>3</sup>) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; (because T(n) = &Theta;(1))<br>
Therefore,; the complexity of T(n) is O(n<sup>3</sup>)

### Substitution Method(대입법)
This is a method calculating T(n) by using the result of repetition method. For example, there is T(n) = 2\*T(n<sup>0.5</sup>) + lgn, where n > 1 and T(2) = 0. Then, if we say n = 2<sup>k</sup>, <br>
T(n) = T(2<sup>k</sup>) = 2\*T((2<sup>k</sup>)<sup>0.5</sup>) + lg2<sup>k</sup><br>
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; = 2\*T(2<sup>0.5k</sup>) + k<br>
Now, let's say T(2<sup>k</sup>) = S(k). Then,<br>
T(n) = T(2<sup>k</sup>)<br>
&nbsp; &nbsp; &nbsp; &nbsp; = S(k) = 2\*S(k/2) + k<br>
This is the same form as T(n) = 2T(n/2) + n = nlgn<br>
Therefore, S(k) = k\*lgk
When T(2) = 0, then k = lgn. If we substitute this value,
S(lgn) = lgn\*lg(lgn)
Therefore, T(n) = lgn\*lg(lgn)

### The Master Method(일반법)
This method is good to know since we can easily get the result.
<br>
Let's think of a form
T(n) = p\*T(n/q) + f(n)
where q >= 1 and p > 1.
<br><br>

if(degree of n<sup>log<sub>q</sub>p</sup> ** < ** degree of f(n)) &nbsp; &nbsp; T(n) = &Theta;(f(n))<br>
else if(degree of n<sup>log<sub>q</sub>p</sup> ** > ** degree of f(n)) &nbsp; &nbsp; T(n) = &Theta;(n<sup>log<sub>q</sub>p</sup>)<br>
else if(degree of n<sup>log<sub>q</sub>p</sup> ** == ** degree of f(n)) &nbsp; &nbsp; T(n) = &Theta;(f(n)\*log<sub>q</sub>n) || T(n) = &Theta;(n<sup>log<sub>q</sub>p</sup>\*log<sub>q</sub>n)

<br>
* Especially you need to consider the number of accessed elements when it is the third case.