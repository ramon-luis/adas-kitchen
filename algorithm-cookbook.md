# Algorithm Cookbook  

[Sorting](#sorting-algorithms)  
[Search and Select](#search-and-select-algorithms)  
[Dynamic Programming](#dynamic-programming-algorithms)  
[Graphs](#graph-algorithms)  


## Sorting Algorithms  
[Insertion Sort](#insertion-sort)  


__Additional Resources for Sorting Algorithms__  
* VisuAlgo: https://visualgo.net/en/sorting  
* Toptal: https://www.toptal.com/developers/sorting-algorithms  

### Insertion Sort  
__Input:__ unsorted array `A[1...n]` of size n  
__Output:__ sorted array `A[1...n]`  
__Structure:__ uses __two indices__ (i and j) and __two loops__: `for j=2 to n, while i>0 and A[i]<key`  
__Run-time:__ O(n<sup>2</sup>)  
__Maintains:__ `key=A[j]` current value that is being inserted (i.e. sorted)  

__Proof__ 
>__Loop Invariant:__ at start of each `for` loop all elements in `A[1...j-1]` are sorted  
>  
>__Base Case:__ at start `j = 2` and `A[1...j-1] = A[1]` &#8756; single element is sorted  
>   
>__Induction Hypothesis (IH):__ assume true for `j` when <code>2 &#8804; j &#8804; k</code>  
>  
>__Inductive Step (IS):__ Consider when `j = k+1`. Item `j = k+1` is only inserted at `A[k+1]` if <code>A[j] &#8805; A[k]</code>. By IH `A[1...k]` is sorted, so this maintains sorted order. Otherwise if <code>A[j] &#8804; [k]</code>, then each element in `A[1...k]` is examined in reverse order until the i<sup>th</sup> element is found where `A[k+1=j] > A[i]`. In the resulting array, `A[1...i]` is sorted, `A[i+1] = A[j]`, and `A[i+2...j-1]` is sorted. Therefore, sorted order is maintained.


#### Pseudocode for Insertion Sort  
```
Insertion-Sort(A[1..n])
01 for j=2 to n
02	key = A[j]
03	i = j-1
04	while i > 0 and A[i] > key
05		A[i+1] = A[i]
06		i = i - 1
07	A[i+1] = key
```

#### Java for Insertion Sort  
```
[code here]

```
__Additional Resources for Insertion Sort__  
* Wikipedia: https://en.wikipedia.org/wiki/Insertion_sort  
* GeeksforGeeks: http://www.geeksforgeeks.org/insertion-sort/  
* Toptal: https://www.toptal.com/developers/sorting-algorithms/insertion-sort  