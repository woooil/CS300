# Ch. 6 Lower Bound

## Complexity of Problems
* $T_A (X)$: Running time of alg $A$ given input $X$
* $T_A (n) := \max_{|X|=n} T_A(X)$
  * The worst running time of $A$ as a function of the input size $n$
* $T_\Pi (n) := \min_{A ~\text{solves}~ \Pi} T_A (n) = \min_{A ~\text{solves}~ \Pi} \max_{|X|=n} T_A(X)$
  * The worst-case running time of the *fastest* algorithm for solving a problem $\Pi$
* "$A$ solves $\Pi$" $\rightarrow$ $T_\Pi (n) \leq T_A(n)$
* $T_\Pi (n) = \Omega(f(n))$ $\rightarrow$ $\forall A ~\text{solves}~ \Pi,~ T_A(n) = \Omega(f(n))$

## Decision Tree Model
* Internal node: Labeled by a question about the input
* Edges: Possible answers to the query
* Leaf: Output
* If the problem has $N$ different outputs, then the decision tree must have at leas $N$ leaves

### Decision Tree for Comparison Sorting
* Internal node: $i:j$
* Comparison: $a_i \leq a_j$
  * If true, then go to the left subtree 
  * If false, then go to the right subtree
* Leaf: Permutation $<\pi(1), \pi(2), ..., \pi(n)>$
* Running time of the algorithm: The length of the path taken
* Worst-case running time: The height of the tree

## Lower Bounds for Comparison Sorting
* $\Omega(n)$ to examine all the input
* All sorts seen so far are $\Omega(n \lg n)$
* Theorem: Any comparison sort alg requires $\Omega(n \lg n)$ comparisons in the worst case
  * There are $l \geq n!$ leaves on the tree; every permutation appears at least once
  * The height of the tree is $h \geq \lg(n!) = \Theta(n \lg n)$ (Stirling's approx.)
  * $h = \Omega(n \lg n)$
* Corollary: Heapsort and merge sort are asymptotically optimal comparison sorts
  * So does randomized quicksort