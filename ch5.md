# Ch. 5 Quicksort

## Quicksort
* Sort an $n$-element array
  
### Alg
* Divide: Partition the array into two subarrs around a pivot $x$ s.t. elems in lower subarr $\leq x \leq$ elems in upper subarr
* Conquer: Recursively sort the two subarrs
* Combine: Trivial

```
Partition(A, p, q)
  x <- A[p]
  i <- p
  for j <- p + 1 to q do
    if A[j] <= x do
      i <- i + 1
      exchange(A[i], A[j])
  exchange(A[p], A[i])
  return i
```

* Invariant
  * `A[p] == x`
  * `A[p + 1...i] <= x`
  * `A[i + 1..j] >= x`

```
Quicksort(A, p, r)
  if p < r do
    q <- Partition(A, p, r)
    Quicksort(A, p, q - 1)
    Quicksort(A, q + 1, p)

Quicksort(A, 1, n)
```

### Analysis
* Worst-case
  * $T(n) = T(0) + T(n - 1) + \Theta(n)$
    * $= T(n - 1) + \Theta(n)$
    * $= \Theta(n^2)$
* Best-case
  * $T(n) = 2T(n/2) + \Theta(n)$
    * $= \Theta(n \lg(n))$
* Partitioning evenly is important
* If the split is always $0.1 : 0.9$
  * $T(n) = T((1/10)n) + T((9/10) n) + \Theta(n)$
  * $T(n) = \Theta(n \lg (n))$ by Recursion Tree method

## Randomized Quicksort
* To avoid the worst case
* Partition around a random element
* Running time is independent of the input order
* The worst case is determined only by the output of a random-number generator

### Analysis
* $T(n)$: Random vairable for the running time on an input of size $n$, assuming random numbers are indep
* $X_k$: Indicator random variable, $k = 0, 1, ..., n-1$
  * $
    X_k = 
    \begin{cases}
      1 & \text{if \texttt{Partition} generates a}~ k : n-k-1 ~\text{split} \\
      0 & \text{otherwise}
    \end{cases}
    $
  * $E[X_k] = P\{X_k = 1\} = 1/n$
* $
  T(n) =
  \begin{cases}
    T(0) + T(n-1) + \Theta(n) & \text{if}~ 0:n-1 ~\text{split} \\
    T(1) + T(n-2) + \Theta(n) & \text{if}~ 1:n-2 ~\text{split} \\
    \vdots \\
    T(n-1) + T(0) + \Theta(n) & \text{if}~ n-1:0 ~\text{split} \\
  \end{cases}
  $ \
  $= \sum_{k = 0}^{n - 1} X_k (T(k) + T(n - k - 1) + \Theta(n))$
* $E[T(n)] = E\left[ \sum_{k = 0}^{n - 1} X_k (T(k) + T(n - k - 1) + \Theta(n)) \right]$
  * $= (2/n)\sum_{k=2}^{n-1} E[T(k)] + \Theta(n)$
* $E[T(n)] \leq a n \lg(n)$ for sufficiently large $a > 0$
  * Can be proven by Substitution method