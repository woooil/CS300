# Ch. 7 Selection

## Selection
* Input: A list of numbers $S$, an integer $k$
* Output: The $k$th smallest element of $S$
* Naive algorithm: Sort $S$ with $O(n \log n)$
  
## Randomized Divide-and-Conquer Algorithm

```
Rand-Select(A, p, q, i)
  if p = q do
    return A[p]
  r <- Rand-Partition(A, p, q)
  k <- r - p + 1
  if i = k do
    return A[r]
  if i < k do
    return Rand-Select(A, p, r - 1, i)
  else do
    return Rand-Select(A, r + 1, q, i - k)
```

### Analysis
* Worst-case
  * $T(n) = T(n - 1) + \Theta(n) = \Theta(n^2)$
* Best-case
  * $T(n) = T(n/2) + O(n) = O(n)$
* Average-case
  * "A pivot $p$ is *good*" $\equiv$ \
    "$p$ lies within the 25th to 75th percentile of the arr"
  * A good pivot reduces the size of the subarr to at most $3/4$ of the size of the array
  * (Time taken on an arr of size $n$) $\leq$ (time taken on an arr of size $3n4$) + (time to reduce arr size to $\leq 3n/4$)
  * $T(n) \leq T(3n/4) + O(n)$

#### Analysis of Expected Time 
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
    T(\max\{n-1\}) + \Theta(n) & \text{if}~ 0:n-1 ~\text{split} \\
    T(\max\{n-2\}) + \Theta(n) & \text{if}~ 1:n-2 ~\text{split} \\
    \vdots \\
    T(\max{n-1, 0}) + \Theta(n) & \text{if}~ n-1:0 ~\text{split} \\
  \end{cases}
  $ \
  $= \sum_{k = 0}^{n - 1} X_k (T(\max\{k, n - k - 1\}) + \Theta(n))$
* $E[T(n)] = (2/n)\sum_{k=\lfloor n/2 \rfloor}^{n-1} E[T(k)] + \Theta(n)$
* $E[T(n)] \leq cn$ for sufficiently large $c > 0$
  * Can be proven by Substitution method
* The worst case: $\Theta(n^2)$

### Worst-Case Linear-Time Selection Algorithm
```
Select(a[1..n], k):
  if n <= 25 do                         
    use brute force
  else do
    m <- ceil(n / 5)                    // Divide n elems into groups of 5
    for i <- 1 to m do
      B[i] <- Select(A[5i - 4...5i], 3) // Find the median of each 5-element group
    mom <- Select(B[1...m], mom)        // Recursively Select the median mom to be the pivot
    if k < r do
      return Select(A[1..r - 1], k)
    else if k > r do
      return Select(A[r + 1..n], k - r)
    else
      return mom
```
* Two subarrs partioned cannot be too large or too small
* The median of group medians ($mom$) is larger than $\lceil \lceil n / 5 \rceil /2 \rceil - 1 \approx n / 10$
* $mom$ is larger than $3n / 10$ elements
* In the worst case, $7n/10$ elements arr need to be searched
* $T(n) \leq T(n / 5) + T(7n / 10) + O(n) = O(n)$