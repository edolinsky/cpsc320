# Assignment 4

Reid Oliveira & Erik Dolinsky

## 1 I'm a Lumberjack (And I'm Okay)

### 1.1 Root-finding

[Answer]

### 1.2 Array-chopping

[Answer]

## 2 Tiles and Tribulations

### 2.0 Warming up

[Answer]

### 2.1 Whether-proof tiles

[Answer]

### 2.2 A million little quadrants

[Answer]

### 2.3 More whether-proofing

[Answer]

## 3 Greed is Good, But Conquest is Better

### 3.1 BF Bank?

[Answer]

### 3.2 D + C = Profit

[Answer]

## 4 Debug-and-Conquer

### 1

#### a)
The best case for this algorithm is a graph which is minimally connected, for
example a tree or chain. In this case, there are only _m = n - 1_ edges, and so
the work done within each call to subgraphs, O(m+n) is O(n). As the rest of the
non-recursive work done in each call to `DC_MST` is at most linear, the work done
within each node is O(n).

#### b)
The worst case for this algorithm is a graph which is fully connected. In this
case, there are _m = n(n-1)/2_ edges, which means that the work done within each
call to subgraphs, O(m+n) is O(n<sup>2</sup>). Asthe rest of the non-recursive
work done in each call to `DC_MST` is at most linear, the work done within each
node is O(n<sup>2</sup>).

### 2

#### a)
As the size of each vertex set produced by `subgraphs()` differ by at most one,
and two graphs are produced, we can say that the two subgraphs produced are
each approximately half the size of the original problem. As both are recursed
upon, and in the best case we have O(n) work at each node (as established in _1a)_),
we have the following recurrence relation:

_T(n) = 2T(n/2) + n_

The Master Theorem states that if for some constant _k >= 0_, and recurrence relation

_T(n) = aT(n/b) + f(n)_ where _a >= 1_ and _b > 1_

_f(n) &#8712; &Theta; (n<sup>c</sup>(log(n))<sup>k</sup>)_, where _c = log<sub>b</sub>a_,
then

_T(n) &#8712; &Theta; (n<sup>c</sup>(log(n))<sup>k+1</sup>)_

Here, we have _a = 2_, _b = 2_, _c = log<sub>2</sub>2 = 1_, and _f(n) = n_.

So we must show _n &#8712; &Theta; (n<sup>1</sup>(log(n))<sup>k</sup>)_. Here,
choose _k = 0_. Then, _n &#8712; &Theta; n_. So, _T(n) &#8712; &Theta; (nlog(n))_,
which also defines the tight lower bound and thus _T(n) &#8712; &Omega; (nlog(n))_.

#### b)
By the same reasoning as in _2a)_, but with the worst-case work per node as
determined in _1b)_, we have the following recurrence relation:

_T(n) = 2T(n/2) + n<sup>2</sup>_

The Master Theorem states that if for some recurrence relation

_T(n) = aT(n/b) + f(n)_ where _a >= 1_ and _b > 1_

If _f(n) &#8712; &Omega;(n<sup>c</sup>)_ where _c > log<sub>b</sub>a_, and
_af(n/b) <= kf(n)_ for some constant _k < 1_ and and sufficiently large _n_,
then

_T(n) &#8712; &Theta; (f(n))_

We have _a = 2_, _b = 2_, _log<sub>2</sub>2 = 1_, and _f(n) = n<sup>2</sup>_.

First, we have _n<sup>2</sup> &#8712; &Omega; (n<sup>c</sup>)_ for _c > 1_.
Choose _c = 2_, then we have _c > 1_, and _n<sup>2</sup> &#8712; &Omega; (n<sup>2</sup>)_
for all _n_.

Next, substituting our values of _a_, _b_, and _f(n)_ into _af(n/b) <= kf(n)_,
we have _2(n/2)<sup>2</sup> <= kn<sup>2</sup>_ for _k < 1_ and sufficiently large _n_.
Simplified, we have _n<sup>2</sup>/2 <= kn<sup>2</sup>_, and thus with _k = 1/2_,
the expression holds for all _n_.

As both expressions are satisfied, we have _T(n) &#8712; &Theta; n<sup>2</sup>_,
and thus the tight upper bound _T(n) &#8712; O(n<sup>2</sup>)_.

### 3

This algorithm does not always generate a minimum spanning tree. Its particular
weaknesses lie in that `subgraphs()` does not strategically pick a cut, and it
does not explore both outcomes in the case that there are two edges with the
same minimum weight.

Consider the following graph G, with labelled vertices and edge weights:

![Imgur](http://i.imgur.com/28q3Ynk.png)

In this case, if `subgraphs(G)` partitions the graph into subgraphs H (V<sub>H</sub> = {A, B})
and I (V<sub>I</sub> = {C, D}) using the red-line cut, then we have two edges
with minimum weight, (A, C) and (B, C). As we have no mechanism for deciding
between the two, we "arbitrarily" (for the sake of argument) pick (A, C) as our
minimum-weight edge, as shown below:

![Imgur](http://i.imgur.com/RkobyV0.png)

Now, within subgraphs H and I, we have only one edge choice each, so we choose
edge (A, B) within subgraph H (weight 2) and edge (C, D) within subgraph I
(weight 4):

![Imgur](http://i.imgur.com/v8sXaWs.png)

The resulting tree is shown below, with total weight 7.

![Imgur](http://i.imgur.com/fSpKQu7.png)

This is a spanning tree, however it is not the minimum spanning tree. It is also
worthy to note that had we picked edge (B, C) instead of (A, C), the result
(A-B-C-D, total weight 7) would still not be the minimum spanning tree.

The minimum spanning tree in this example is shown below, with total weight 4.

![Imgur](http://i.imgur.com/vYIWcdz.png)

This could be obtained by first cutting G into graphs J (V<sub>J</sub> = {A})
and K (V<sub>K</sub> = {B, C, D}), and then K into graphs L (V<sub>L</sub> = {D})
and M (V<sub>M</sub> = {B, C}).
