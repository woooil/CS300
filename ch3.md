# Ch. 3 Recurrence

## Divide-and-Conquer Design Alg
1. Divide: Divide a problem into subproblems
2. Conquer: Solve the subproblems *recursively*
3. Combine: Combine the subproblem solutions appropriately

## Merge Sort
1. Divide: Divide the input arr in 2 subarrs
2. Conquer: Recursively sort 2 subarrs
3. Combine: Merge 2 sorted subarrs

```
    function mergesort(a[1...n])
    Input: An arr of numbers a[1...n]
    Output: A sorted version of this array

    if n > 1 do
      return merge(mergesort(a[1...floor(n/2)]), mergesort(a[floor(n/2) + 1...n]))
    else do
      return a
```
```
    function merge(x[1...k], y[1...l])
      if k = 0 do
        return y[1...l]
      if l = 0 do
        return x[1...k]
      if x[1] <= y[1] do
        return x[1] + merge(x[2...k], y[1...l])
      else do
        return y[1] + merge(x[1...k], y[2...l])
```

### Analysis
* $T(n) = 2T(n / 2) + \Theta(n)$
  * $2$: The number of subproblems
  * $T(n / 2)$: The size of subproblem
  * $\Theta(n)$: Time for dividing($\Theta(1)$) and combining($\Theta(n)$)
  
## Recurrences
* We get a recur for algs expressed in a recur way
* Basic cases
  * When the problem size is small, use an approach that takes constant time
  * $T(n) \leq c$ for $\forall n \leq n_0$
  
## 4 Techs for Solving Recurrences
* Solve by unrolling
* Substitution method
* Recursion tree
* Master method

## Solve by Unrolling
### Selection sort
* Find the smallest elem and put it in the leftmost position ($\Theta(n)$) Then, recursively sort the remainder of the array ($T(n - 1)$)
* $T(n) = cn + T(n - 1) = cn + c(n - 1) + c(n - 2) + ... + c$
* Each of top $n / 2$ terms $\geq cn / 2$
* $(n/2)(cn/2) \leq T(n) \leq cn^2$
* $T(n) = \Theta(n^2)$

## Substitution Method
1. Guess the form of the sol
2. Use methmatical induction to find the constants and show that the solution works
* Show the upper and lower bounds separately.
* Show the *exact* form of the I.H. 

### $T(n) = 4T(n/2) + n$
* Assume $T(1) = \Theta(1)$
* Guess $T(n) = O(n^3)$
* Assume $T(k) \leq ck^3$ for $k<n$
* Prove $T(n) \leq cn^3$ by induction
  * $T(n) = 4T(n / 2) + n$
    * $\leq 4c(n/2)^3 + n$
    * $= (c/2)n^3 + n$
    * $= cn^3 - ((c/2)n^3 - n)$
    * $\leq cn^3$ if $(c/2)n^3 - n \geq 0$
* Need to handle the initial conditions
  * Base: $T(n) = \Theta(1)$ for $\forall n<n_0$, a suitable constant $n_0$
  * For $1 \leq n < n_0$, $\Theta(1) \leq cn^3$, if $c$ is big enough
* This bound is not tight

### Trial 1 for Tighter Bound
* If we guess $T(n) = O(n^2)$...
* Assume $T(k) \leq ck^2$ for $k<n$
* $T(n) = 4T(n/2) + n$
  * $\leq 4c(n/2)^2 + n$
  * $=cn^2 + n$
  * $\neq O(n^2)$
* We must prove the exact form of the I.H. $\leq cn^2$
* Strenthen the I.H. by subtracting a low-order term

### Trial 2 for Tighter Bound
* Strenthen the I.H. by subtracting a low-order term
* Assume $T(k) \leq c_1 k^2 - c_2 k$ for $k < n$
* $T(n) = 4T(n /2) + n$
  * $\leq 4(c_1 (n/2)^2 - c_2 (n/2)) + n$
  * $= c_1 n^2 - 2 c_2 n + n$
  * $=c_1 n^2 - c_2 n -(c_2 n - n)$
  * $\leq c_1 n^2 - c_2 n$ if $c_2 > 1$
* Initial conditions: Pick $c_1$ big enough

## Recursion-Tree Method
* Provides a good guess for the substitution method
* Node: The cost of a single problem
* Per-level costs: Sum the costs within each level 
* The total cost: Sum all the per-level costs

### $T(n) = T(n/4) + T(n/2) + n^2$
* Per-level costs
  * $n^2$, $(5/16) n^2$, $(25/256) n^2$, ...
* Total cost
  * $n^2 (1+ 5/16 + (5/16)^2 + (5/16)^3 + ...)$
  * $=\Theta(n^2)$

### Divide-and-Conquer Style Recurrence
* $T(n) = aT(n/b) + cn^k$
  * $
  =
  \begin{cases}
    \Theta(n^k) & \text{if}~ a<b^k ~\text{(root-dominant)} \\
    \Theta(n^k \log(n)) & \text{if}~ a=b^k & \\
    \Theta(n^{\log_b {a}}) & \text{if}~ a>b^k ~\text{(leaf-dominat)}
  \end{cases}
  $

## Master Method
* $T(n) = a T(n / b) + f(n)$, $T: \R^* \rightarrow \R$ for $a \geq 1$, $b > 1$, $f(n) > 0$
  * Root: $f(n)$
  * Leaves: $n^{\log_{b}{a}}$
1. Case 1: $f(n) = O(n ^{\log_{b}{a - \epsilon}})$ for some $\epsilon > 0$
    * $T(n) = \Theta(n^{\log_b a})$
    * Leaf-dominant
2. Case 2: $f(n) = \Theta(n^{\log_{b}{a}} \lg^{k}{n})$ for some $k \geq 0$
    * $T(n) = \Theta(n^{log_{b}{a}} \lg^{k + 1} n)$
3. Case 3: $f(n) = \Omega(n^{\log_{b}{a + \epsilon}})$ for some $\epsilon > 0$ and $f(n)$ satisfies the regularity condition
    * Regularity condition: $af(n/b) \leq cf(n)$ for $c<1$ and all sufficiently large $n$
    * $T(n) = \Theta(f(n))$
    * Root-dominant

