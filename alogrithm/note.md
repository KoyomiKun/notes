# Introduction of Alogrithm

## Recursion

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

## Divide and Conquer

devide -> conquer -> combine

***Ex: Merge Sort*** 

1. divide the array into 2 sub array

2. sort each sub array

3. combine the solution

Running Time
> $T(n) = 2T(\frac{n}{2}) + O(n)$ 

**Q : Why the upperbound can be ignore**


### Master Method

compare f(n) with $nlog_ba$

Case 1: f(n) is smaller, $f(n) = O(n^{log_ba-\epsilon}) for\ some\ \epsilon >0$ -> $T(n) = \Theta(n^{log_ba})$ 

Case 2: f(n) is equal, $f(n) = \Theta (n^{log_ba log^kn}) for\ some\ k\ge 0$ -> $T(n)= \Theta(n^{log_ba} log^{k+1}n)$ 

Case 3: f(n) grows more quicker, $f(n) = \Omega(n^{log_b{a+\epsilon}}) for\ some\ \epsilon >0$ ***AND*** $af(\frac nb)\le (1-\epsilon')\cdot f(n)$ for some $\epsilon' >0$ (top of the tree must be bigger than the sum of the values of next level down) -> $T(n) = \Theta(f(n))$ 

 




