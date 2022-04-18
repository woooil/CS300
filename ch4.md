# Ch. 4 Divide-and-Conquer

## Multiplication
* Multiply two $n$-bit numbers

### Naive Idea
* $41 \times 42$
  * Add $41$ to itself $42$ times
  * $\Theta(2^n)$

### Divide-and-Counquer Mul
* $X = 2^{n/2}A + B$, $Y = 2^{n/2}C + D$
* $XY = 2^n AC + 2^{n/2} BC + 2^{n/2} AD + BD$
* $T(n) = 4T(n/2) + cn$
  * $cn$: Time for dividing and combining
* Karatsuba algorithm
  * $XY = (2^n - 2^{n2}) AC + 2^{n/2} (A + B) (C + D) + (1 - 2^{n/2}) BD$
  * $T(n) = 3T(n/2) + c' n$

## Matrix Mult
### Divide-and-Conquer Alg
* $n \times n$ matrix $\rightarrow$ $2 \times 2$ matrix of $(n/2) \times (n/2)$ submatrices
* $
  A =
  \begin{bmatrix}
    a & b\\
    c & d\\
  \end{bmatrix}
  $, 
  $
  B =
  \begin{bmatrix}
    e & f\\
    g & h\\
  \end{bmatrix}
  $
* $C = A \times B = 
  \begin{bmatrix}
    r & s\\
    t & u\\
  \end{bmatrix}
  $
  
  * $
    \begin{cases}
      r = ae + bg \\
      s = af + bh \\
      t = ce + dg \\
      u = cf + dh
    \end{cases}
    $
* $8$ muls of $(n/2) \times (n/2)$ submats
* $4$ muls of $(n/2) \times (n/2)$ submats
* Analysis
  * $T(n) = 8 T(n/2) + \Theta(n^2)$
    * $8$: The number of submats
    * $n/2$: The size of submats
    * $\Theta(n^2)$: Time for adding submats

### Strassen's Idea
* Multiply $2 \times 2$ matrices w/ only $7$ rec muls
* $
  \begin{cases}
    r = P_5 + P_4 - P_2 + P_6 \\
    s = P_1 + P_2 \\
    t = P_3 + P_4 \\
    u = P_5 + P_1 + P_3 - P_7
  \end{cases}
  $
  * $
    \begin{cases}
      P_1 = a (f - h) \\
      P_2 = (a + b) h \\
      P_3 = (c + d) e \\
      P_4 = d (g - e) \\
      P_5 = (a + d) (e + h) \\
      P_6 = (b - d) (g + h) \\ 
      P_7 = (a - c) (e + f)
    \end{cases}
    $
* $T(n) = 7T(n/2) + \Theta(n^2)$

## Binary Search
* Alg
  * Divide: Check middle element
  * Conquer: Recursively search $1$ subarray
  * Combine: Trivial
* Analysis
  * $T(n) = 1 T(n/2) + \Theta(1)$
    * $1$: The number of subproblems
    * $n/2$: The size of subproblems
    * $\Theta(1)$: Time for dividing and combining

## Powering a Number
* Compute $a^n$ where $n \in \N$
* $\Theta(n)$?

### Divide-and-Conquer
* $
  a^n = 
  \begin{cases}
    a^{n/2} \cdot a^{n/2} & \text{if}~ n ~\text{is even} \\
    a^{(n - 1)/2} \cdot a^{(n - 1)/2} \cdot a & \text{if}~ n ~\text{is odd}
  \end{cases}
  $
* Analysis
  * $T(n) = T(n/2) + \Theta(1)$
  * $T(n) = \Theta(\lg (n))$

## Fibonacci Number
* $
  F_n = 
  \begin{cases}
    0 & \text{if}~ n=0 \\
    1 & \text{if}~ n=1 \\
    F_{n-1} + F_{n-2} & \text{if}~ n \geq 2
  \end{cases}
  $

### Recursive Squaring
* $
  \begin{bmatrix}
   F_{n + 1} & F_n \\
   F_n & F_{n - 1}
  \end{bmatrix} =
  \begin{bmatrix}
    1 & 1 \\
    1 & 0
  \end{bmatrix} ^ n
  $
* Can be proven by induction