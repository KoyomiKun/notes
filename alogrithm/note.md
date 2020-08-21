# Introduction of Alogrithm

## 1. Recursion

**Wrong Proof** 

> n = O(1)
>
> proof : 
> 
> 1. 1 = O(1)
> 
> 2. assume n-1 = O(1)
>
> 3. n = n-1 + 1 so n is O(1)

it seems right, but actually not. Because although O(1) is a `constant` value, the constant is varible. it means this constant has function ralation with `n`  

## 2. Divide and Conquer

devide -> conquer -> combine
### 2.1 Ex: Merge Sort

1. divide the array into 2 sub array

2. sort each sub array

3. combine the solution

Running Time
> $T(n) = 2T(\frac{n}{2}) + O(n)$ 

**Q : Why the upperbound can be ignore**
### 2.2 Ex: Binary Search

$T(n) = T(n/2) + O(1)$ solve it with master method:

$T(n) = O(logN)$ 

### 2.3 Ex: Fibonacci Num
$$
\left[
\begin{matrix}
F_{n-1} & F_n\\
F_{n} & F_n\\
\end{matrix}
\right]
=
\left[
\begin{matrix}
1 & 1\\
1 & 0
\end{matrix}
\right]
$$
$O(log(N))$ 

code in `~/scripts/fibonacci`  
Proof by induction

### 2.4 Stressen's alogrithm

**Problem** : how to compute two square matrixs in size n x n

**Simple DC** : divide a matrix into 4 submatrixs in n//2 x n//2

**Result** : $T(n) = 8T(n/2) + \Theta(n^2) = \Theta(n^3)$, not work 

**Stressen's idea** : reduce multiplications


### 2.1 Master Method

compare f(n) with $nlog_ba$

Case 1: f(n) is smaller, $f(n) = O(n^{log_ba-\epsilon}) for\ some\ \epsilon >0$ -> $T(n) = \Theta(n^{log_ba})$ 

Case 2: f(n) is equal, $f(n) = \Theta (n^{log_ba log^kn}) for\ some\ k\ge 0$ -> $T(n)= \Theta(n^{log_ba} log^{k+1}n)$ 

Case 3: f(n) grows more quicker, $f(n) = \Omega(n^{log_b{a+\epsilon}}) for\ some\ \epsilon >0$ ***AND*** $af(\frac nb)\le (1-\epsilon')\cdot f(n)$ for some $\epsilon' >0$ (top of the tree must be bigger than the sum of the values of next level down) -> $T(n) = \Theta(f(n))$ 

## Quick Sort






