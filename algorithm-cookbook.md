# Algorithm Cookbook  
[Runtime](#runtime)  
[Sorting Algorithms](#sorting-algorithms)  
&nbsp;&nbsp;[Insertion Sort](#insertion-sort)  
&nbsp;&nbsp;[Merge Sort](#merge-sort)  
&nbsp;&nbsp;[Merge](#merge)  
&nbsp;&nbsp;[Quicksort](#quicksort)  
&nbsp;&nbsp;[Partition](#partition)  
[Search and Select Algorithms](#search-and-select-algorithms)  
&nbsp;&nbsp;[Binary Search](#binary-search)  
&nbsp;&nbsp;[Inversion Count](#inversion-count)  
&nbsp;&nbsp;[Triple Count](#triple-count)  
&nbsp;&nbsp;[k From Median](#k-from-median)
&nbsp;&nbsp;[Array Contains Integers That Sum to X](#array-contains-integers-that-sum-to-x)  
&nbsp;&nbsp;[k Largest Less Than Median](#k-largest-less-than-median)  
[Dynamic Programming Algorithms](#dynamic-programming-algorithms)  
[Graph Algorithms](#graph-algorithms)  

## Runtime

### Master Theorem for Runtime on Recursive Algorithms

__T(n) = a * T(<sup>n</sup>&frasl;<sub>b</sub>) + O(n<sup>d</sup>)__  

1. Identify `a`, `b`, &ambp; `d`  
2. Apply table below to solve for runtime  

Value of d | T(n)  
--- | ---  
d > log<sub>b</sub> a | O(n<sup>d</sup>)  
d = log<sub>b</sub> a | O(n<sup>d</sup> lg n) where lg = log<sub>2</sub>  
d < log<sub>b</sub> a | O(n<sup>log<sub>b</sub> a</sup>)  

__Useful to remember:__  
* O(n<sup>0</sup>) = O(1)  
* log<sub>x</sub> x = 1  
* log<sub>x</sub> 1 = 0  
* O(n + a)<sup>b</sup> = O(n)<sup>b</sup>  

## Sorting Algorithms  
[Insertion Sort](#insertion-sort)  
[Merge Sort](#merge-sort)  
[Merge](#merge)  
[Quicksort](#quicksort)  
[Partition](#partition)  

### Insertion Sort  
__Input:__ unsorted array `A[1...n]` of size n  
__Output:__ sorted array `A[1...n]`  
__Structure:__ uses __two indices__ (i and j) and __two loops__: `for j=2 to n, while i>0 and A[i]<key`  
__Maintains:__ `key=A[j]` for current value that is being inserted (i.e. sorted)  
__Run-time:__ O(n<sup>2</sup>)  

#### Proof for Insertion Sort  
>__Loop Invariant:__ at start of each `for` loop all elements in `A[1...j-1]` are sorted  
>  
>__Base Case:__ at start `j=2` and `A[1...j-1] = A[1]` &#8756; single element is sorted  
>   
>__Induction Hypothesis (IH):__ assume true for `j` when <code>2 &#8804; j &#8804; k</code>  
>  
>__Inductive Step (IS):__ Consider when `j = k+1`. Item `j = k+1` is only inserted at `A[k+1]` if <code>A[j] &#8805; A[k]</code>. By IH `A[1...k]` is sorted, so this maintains sorted order. Otherwise if <code>A[j] &#8804; [k]</code>, then each element in `A[1...k]` is examined in reverse order until the i<sup>th</sup> element is found where `A[k+1=j] > A[i]`. In the resulting array, `A[1...i]` is sorted, `A[i+1] = A[j]`, and `A[i+2...j-1]` is sorted. Therefore, sorted order is maintained.

#### Pseudocode for Insertion Sort  
<pre><code>
<b>Insertion-Sort(A[1..n])</b> where A is <b>unsorted</b> array
01 for j=2 to n
02	key = A[j]
03	i = j-1
04	while i > 0 and A[i] > key
05		A[i+1] = A[i]
06		i = i-1
07	A[i+1] = key
</pre></code>

#### Java for Insertion Sort  
```
[code here]

```
__Additional Resources for Insertion Sort__  
* Wikipedia: https://en.wikipedia.org/wiki/Insertion_sort  
* GeeksforGeeks: http://www.geeksforgeeks.org/insertion-sort/  
* Toptal: https://www.toptal.com/developers/sorting-algorithms/insertion-sort  

### Merge Sort  
__Input:__ unsorted array `A[1...n]`, left index `p`, right index `r`  
__Output:__ sorted array `A[1...n]`  
__Structure:__ recursion, split array in half until single itm, then merge back together in sorted order  
__Maintains:__ <code>q = &#8970;<sup>p+r</sup>&frasl;<sub>2</sub>&#8971;</code> for middle index of array  
__Run-time:__ O(n lg n) where lg = log<sup>2</sup>  

<blockquote>
Recursive Call: 2 (<sup>n</sup>&frasl;<sub>2</sub>)<br />
Merge Call: O(n)<br />
Total Runtime: T(n) = 2 (<sup>n</sup>&frasl;<sub>2</sub>) + O(n)<br />
Reduce: apply Master Theorem where a=2, b=2, d=1 to arrive at O(n lg n) where lg = log<sup>2</sup>
</blockquote>  

#### Pseudocode for Merge Sort  
<pre><code>
<b>Merge-Sort(A[1..n], p, r])</b> where A is <b>unsorted</b> array
01 if p &lt; r
02	q = &#8970;<sup>p+r</sup>&frasl;<sub>2</sub>&#8971;
03	MergeSort(A, p, q)
04	MergeSort(A, q+1, r)
05	Merge(A, p, q, r)
</code></pre>

#### Java for Merge Sort  
```
[code here]

```
__Additional Resources for Merge Sort__  
* Wikipedia: https://en.wikipedia.org/wiki/Merge_sort  
* GeeksforGeeks: http://www.geeksforgeeks.org/merge-sort/  
* Toptal: https://www.toptal.com/developers/sorting-algorithms/merge-sort  

### Merge  
__Input:__  
Either:
  * Two __sorted__ arrays A<sub>1</sub> and A<sub>2</sub> or  
  * Single array with markers: left index `p`, middle index `q`, right index `r`  

__Output:__ single sorted array (input arrays combined)  
__Structure:__  
  1. creates copies of input arrays with sentinels added to each input array  
  2. loops `for k = p to r` and updates `A[k]` to min value of L &amp; R  

__Maintains:__  
  * <code>n<sub>1</sub> = q - p + 1</code>  to track section of input array  
  * <code>n<sub>2</sub> = r - q</code> to track remaining section of input array  
  * <code>L[1...n<sub>1</sub> + 1], R[1...n<sub>2</sub> + 1]</code> for copies of input array with extra data location for sentinels  
  * `i=1, j=1` for markers for location in L &amp; R during (k = p to r) loop  

__Run-time:__ O(n) // 3 `for` loops: n<sub>1</sub>, n<sub>2</sub>, `p to r = n`

#### Pseudocode for Merge  
<pre><code>
<b>Input: Single array with markers to sub-divide</b>
<b>Merge(A, p, q, r])</b> where A is <b>unsorted</b> array
01 n<sub>1</sub> = q - p + 1  // p to q <em>inclusive</em> of q
02 n<sub>2</sub> = r - q  // q to r <em>exclusive</em> of q
03 L[1...n<sub>1</sub>+1], L[1...n<sub>2</sub>+1] are new arrays  // need extra memory allocation for sentinels
04 for i = 1 to n<sub>1</sub>  // fill L
05 	L[i] = A[p+i-1]
06 for j = 1 to n<sub>2</sub>  // fill R
07 	R[j] = A[q+j]
08 L[n<sub>1</sub>+1] = &#8734;  // add sentinel
09 R[n<sub>2</sub>+1] = &#8734;  // add sentinel
10 i=1, j=1 // reset i &amp; j
11 for k = p to r  // loop and assign min value
12 	if L[i] &#8804; R[j]
13 		A[k] = L[i]
14 		i = i+1
15 	else
16 		A[k] = R[j]
17 		j = j+1
</code></pre>

<pre><code>
<b>Input: Two sorted arrays</b>
<b>Merge(A<sub>1</sub>[1...m], A<sub>2</sub>[1...n])</b> where A<sub>1</sub> and A<sub>2</sub> are <b>sorted</b> arrays
01 L[1...m+1], R[1...n+1], A[1...m+n] are new arrays  // need extra memory allocation for sentinels
02 for i = 1 to m  // fill L
03 	L[i] = A<sub>1</sub>[i]
04 for j = 1 to n  // fill R
05 	R[j] = A<sub>2</sub>[j]
06 L[m+1] = &#8734;  // add sentinel
07 R[n+1] = &#8734;  // add sentinel
08 i=1, j=1 // reset i &amp; j
09 for k = 1 to (m+n)
10 	if L[i] &#8804; R[j]
11 		A[k] = L[i]
12 		i = i+1
13 	else
14 		A[k] = R[j]
15 		j = j+1
16 return A
</code></pre>

#### Java for Merge  
```
[code here]

```
__Additional Resources for Merge Sort__  
* Wikipedia: https://en.wikipedia.org/wiki/Merge_algorithm   
* GeeksforGeeks: http://www.geeksforgeeks.org/merge-two-sorted-arrays/  

### Quicksort  
__Input:__ unsorted array A[1...n], left index p, right index r  
__Output:__ sorted array A[1...n]  
__Structure:__  
  1. Partitions (semi-sorts) last item in array `q`  
  2. Recursively calls itself on lower and upper parts of array aroud `q` (sorted partition element)  
  3. If <b>randomized</b>, then partition call is to `Randomized-Partition`  

__Maintains:__  single element that is in sorted position, marked as either:  
  * `q = Partition(A, p, r)` or  
  * `q = Randomized-Partition(A, p, r)`  

__Run-time:__  
  * Worst Case: O(n<sup>2</sup>) for both non-randomized and randomized  
  * Expected Case: O(n lg n) for <b>randomized only</b> where lg = log<sub>2</sub>  

#### Pseudocode for Quicksort  
<pre><code>
<b>Quicksort(A[1...n], p, r)</b> where A is <b>unsorted</b> array
01 if p &lt; r
02 	q = Partition(A, p, r)  // T(n) = O(n)
03 	Quicksort(A, p, q-1)  // T(n) ranges from O(n lg n) to O(n<sup>2</sup>)
04 	Quicksort(A, q+1, r)  // T(n) ranges from O(n lg n) to O(n<sup>2</sup>)
</code></pre>

<pre><code>
<b>Random-Quicksort(A[1...n], p, r)</b> where A is <b>unsorted</b> array
01 if p &lt; r
02 	q = Random-Partition(A, p, r)  // T(n) = O(n)
03 	Random-Quicksort(A, p, q-1)  // T(n) ranges from O(n lg n) to O(n<sup>2</sup>)
04 	Random-Quicksort(A, q+1, r)  // T(n) ranges from O(n lg n) to O(n<sup>2</sup>)
</code></pre>

<pre><code>
<b>Random-Partition(A, p, r)</b> where A is <b>unsorted</b> array
01 x = Random(p,r)  // random index between p and r inclusive
02 exchange A[x] and A[r]
03 return Partition(A, p, r)
</code></pre>


#### Java for Quicksort 
```
[code here]

```
__Additional Resources for Quicksort__  
* Wikipedia: https://en.wikipedia.org/wiki/Quicksort  
* GeeksforGeeks: http://www.geeksforgeeks.org/quick-sort/  
* Toptal: https://www.toptal.com/developers/sorting-algorithms/quick-sort  

### Partition  
__Input:__ unsorted array A[1...n], left index p, right index r  
__Output:__ the __index__ of sorted pivot element with array _semi-sorted_ around it  
__Structure:__  
  1. Marks pivot element `x = A[r]`  
  2. Marks `i = p-1` at outer left of array  
  3. Loops `for j = p to r-1`  
    * Swaps elements so that <code>A[1...i] &lt; x &lt; A[i+1...j-1]</code>  

__Loop Invariant:__  At the start of each `for` loop:
  * all elements in <code>A[p...1] &#8804; x</code>
  * all elements in <code>A[i+1...j-1] &gt; x</code>
  * all elements in `A[j...r-1]` are unsorted  

**INSERT DIAGRAM HERE**

__Run-time:__ O(n) // loops through each element once

What does Partition return if all elements are equal in value? __i+1 = r__

#### Pseudocode for Partition 
<pre><code>
<b>Partition(A, p, r)</b> where A is <b>unsorted</b> array
01 x = A[r]  // set pivot
02 i = p-1  // set left marker i outside of p
03 for j = p to r-1  // loop from p to pivot
04 	if A[j] &#8804; A[i]
05 		i = i+1
06 		exchange A[i], A[j]
07 exchange A[i+1], A[r]  // swap pivot
08 return i+1  // return pivot index
</code></pre>

#### Java for Partition 
```
[code here]

```  

## Search and Select Algorithms  
[Binary Search](#binary-search)  
[Inversion Count](#inversion-count)  
[Triple Count](#triple-count)  
[k From Median](#k-from-median)  
[Array Contains Integers That Sum to X](#array-contains-integers-that-sum-to-x)  
[k Largest Less Than Median](#k-largest-less-than-median)  

### Binary Search 
__Input:__ a __sorted__ array, value `x` to find  
__Output:__ the __index position__ of value `x` in array  
__Structure:__  checks middle element and recursively searches left or right as appropriate  
__Run-time:__ O(lg n) where lg = log<sub>2</sub>  

__IMPORTANT:__ Always __SORT__ before using Binary Search!  

#### Pseudocode for Binary Search  
<pre><code>
<b>Binary-Search(A, x)</b> where A is a <b>SORTED</b> array
01 return Recursive-Search(A, 1, n, x)  // initial call, assumes 1 is left-most index
</code></pre>

<pre><code>
<b>Recursive-Search(A, p, r, x)</b> where A is a <b>SORTED</b> array, p and r are left and right markers, and x is element to find within A
01 if p == r and A[p] == x  \\ single element in array
02 	return p
03 if p &lt; r
04 	q = &#8970;<sup>p+r</sup>&frasl;<sub>2</sub>&#8971;
05 	if A[q] == x
06 		return q  // found x
07 	if x &lt; A[q]
08 		return Recursive-Search(A, p, q, x)  \\ x is located in L half
09 	else 
10 		return Recursive-Search(A, <b>q+1</b>, r, x)  \\ x is located in R half, careful to use <b>q+1</b>
11 return -1  // x not found
</code></pre>

#### Java for Binary Search 
```
[code here]

```
__Additional Resources for Binary Search__  
* Wikipedia: https://en.wikipedia.org/wiki/Binary_search_algorithm  
* GeeksforGeeks: http://www.cdn.geeksforgeeks.org/binary-search/  
* Khan Academy: https://www.khanacademy.org/computing/computer-science/algorithms/binary-search/a/binary-search  

### Select  
__Input:__ unsorted array A, __order statistic i__ that seeks <b>i<sup>th</sup> smallest element</b> (1 &#8804; i &#8804;
 n)  
__Output:__ the __value__ of the i<sup>th</sup> smallest element (i.e. the element x that is larger than exactly i-1 other elements)  
__Structure:__  
  1. Partitions to semi-sort an element (random or median of medians)  
  2. Partition returns an index:  
      * if the index matches `i` then return that value  
      * else recursively call on upper or lower half as appropriate  

__Run-time:__ 
  * Worst Case for Randomized-Select: O(n<sup>2</sup>)  
  * Expected Case for Randomized-Select: O(n)  
  * Worse Case for Deterministic-Select: O(n)  

#### Pseudocode for Select 
<pre><code>
<b>Random-Select(A, p, r, i)</b> where A is <b>unsorted</b> array
01 if p == r
02 	return A[p]
03 q = Random-Partition(A, p, r)
04 k = q - p + 1  // <b>important: # of elements in (q-p) inclusive (i.e. L half)</b>
05 if k == i  // found i<sup>th</sup> element since A[q] is sorted pivot
06 	return A[q]
07 if i &lt; k  // i<sup>th</sup> element is less than pivot and in L
08 	return Random-Select(A, p, q-1, i)
09 else  // i<sup>th</sup> element is greater than pivot and in R
10 	return Random-Select(A, q+1, r, i-k)  // <b>important: adjust i when searching right</b>
</code></pre>  

<pre><code>
<b>Select(A, i)</b>
01 use median of medians to partition
02 return i<sup>th</sup> smallest value in A
</code></pre>  

#### Java for Select 
```
[code here]

```
__Additional Resources for Select__    
* Wikipedia for QuickSelect:  https://en.wikipedia.org/wiki/Quickselect  
* Wikipedia for Median of Medians: https://en.wikipedia.org/wiki/Median_of_medians  
* GeeksforGeeks: http://www.geeksforgeeks.org/kth-smallestlargest-element-unsorted-array/  

### Inversion Count  
__Input:__ unsorted array A[1...n]  
__Output:__ the number of inversions where A[i] &gt; A[j] and i &lt; j  
__Structure:__  modify Merge-Sort, anything from R before L is empty in an inversion of `(q-p+1) - (i-1)`  
__Run-time:__ O(n lg n) same as Merge Sort  

#### Pseudocode for Inversion Count  
<pre><code>
<b>Inversion-Count(A)</b> where A is <b>unsorted</b> array
01 inversionCount = 0  // global variable
02 Merge-Sort(A, 1, n)  // call Merge Sort on entire array
03 return inversionCount
</code></pre>  

<pre><code>
<b>Merge-Sort(A, p, r)</b>
01 if p &lt; r  // standard algo with call to Merge()
...
...
05 	Merge(A, p, q, r)
</code></pre>  

<pre><code>
<b>Merge(A, p, q, r])</b>  // modified to count inversions
01 n<sub>1</sub> = q - p + 1  // p to q <em>inclusive</em> of q
02 n<sub>2</sub> = r - q  // q to r <em>exclusive</em> of q
03 L[1...n<sub>1</sub>+1], L[1...n<sub>2</sub>+1] are new arrays  // need extra memory allocation for sentinels
04 for i = 1 to n<sub>1</sub>  // fill L
05 	L[i] = A[p+i-1]
06 for j = 1 to n<sub>2</sub>  // fill R
07 	R[j] = A[q+j]
08 L[n<sub>1</sub>+1] = &#8734;  // add sentinel
09 R[n<sub>2</sub>+1] = &#8734;  // add sentinel
10 i=1, j=1 // reset i &amp; j
11 for k = p to r  // loop and count inversions before sorting
12 	if L[i] &gt; R[j] and L[i] != &#8734;  // <b>inversion found</b>
13 		inversionCount = inversionCount + (q-p+1) - (i-1)  // <b>update inversion count</b>
12 	if L[i] &#8804; R[j]  // continue with standard sorting
13 		A[k] = L[i]
14 		i = i+1
15 	else
16 		A[k] = R[j]
17 		j = j+1
</code></pre>  


#### Java for Inversion Count  
```
[code here]

```  

### Triple Count  
__Input:__ an __unsorted__ array A[1...n] consisting of distinct integers  
__Output:__ the count of _triples_ in array where 3 numbers sum to zero  
__Idea:__ sort array, then take every pair of numbers and run __binary search__ to find 3rd value  
__Run-time:__ O(n<sup>2</sup> lg n)  // Binary-Search (lg n) called on double loop (n<sup>2</sup>)

#### Pseudocode for Triple Count  
<pre><code>
<b>Triple-Count(A[1...n])</b> where A is <b>unsorted</b> array
01 Merge-Sort(A)  // need to sort before we can use Binary-Search
02 tripleCount = 0
03 for i = 1 to n-1  // loop through all pairs from left to right
04 	for j = 2 to n
05 		x = A[i]
06 		y = A[j]
07 		z = -(x+y)  // x + y + z = 0, therefore z = -(x+y)
08 		if Binary-Search(A,z) != -1  // Binary-Search found a match, therefore triple exists
09 			tripleCount = tripleCount + 1
10 return tripleCount
</code></pre>  

#### Java for Triple Count  
```
[code here]

```

### k From Median  
__Input:__ an __unsorted__ array A[1...n] and positive integer k &#8804; n  
__Output:__ k numbers closest in __value__ to the median of   
__Idea:__  
  * find median  
  * create new array D to store the absolute distance from median for each element in A (with same index)  
  * copy A to A' and call Select(A', k) to find k<sup>th</sup> value.  
    * need to use A' because Select will semi-sort the array  
  * Check values in D: if &#8804; k<sup>th</sup>, then return original value stored in A  

__Run-time:__  

#### Pseudocode for k From Median  
<pre><code>
<b>K-From-Median(A, k)</b> where A is <b>unsorted</b> array
01 medianIndex = &#8970;<sup>n</sup>&frasl;<sub>2</sub>&#8971;
02 medianValue = Select(A, medianIndex)
03 D is new array
04 for i = 1 to n
05 	D[i] = |A[i] - medianValue|  // distance from median value
06 copy D to K
07 kValue = Select(K, kValue)
08 for i = 1 to n
09 	if D[i] &#8804; kValue
10 		print A[i]
</code></pre>  

#### Java for k From Median  
```
[code here]

```  

### Array Contains Integers That Sum to X  
__Input:__ an __unsorted__ array A[1...n] of distinct integers and target sum integer x  
__Output:__ True/False if array contains two integers that sum to x  
__Idea:__  
  * x = y + z  
  * x = y + A[i]  
  * we are given x and know A[i], therefore need to search for y = x - A[i]  
  * sort, then use Binary-Search  

__Run-time:__  O(n lg n)  // Binary-Search (lg n) called on all elements (n)  

#### Pseudocode for Array Contains Integers That Sum to X  
<pre><code>
<b>Has-Integer-Sum(A[1...n], x)</b> where A is <b>unsorted</b> array
01 Merge-Sort(A)  // <b>important: sort first so we can use BinarySearch</b>
02 for i = 1 to n
03 	y = x - A[i]
04 	if Binary-Search(A, y) != 1  // found a match, T(n) = O(lg n)
05 		return true
06 return false
</code></pre>  

#### Java for Array Contains Integers that Sum to X  
```
[code here]
```  

### k Largest Less Than Median  
__Input:__ an __unsorted__ array A[1...n] of distinct integers and positive integer k &lt; <sup>n</sup>&frasl;<sub>2</sub>  
__Output:__ k largest numbers in A that are less than the median  
__Idea:__  
  * find median  
  * use select to roughly sort and get small values below median  
  * use select to roughly sort again and get m-k values below m-k index  
  * values in m-k to median should be grouped accordingly  

__Run-time:__  O(n)  

#### Pseudocode for k Largest Less Than Median  
<pre><code>
<b>Get-K-Largest-Less-Than-Median(A, k)</b> where A is <b>unsorted</b> array
01 m = &#8970;<sup>n+1</sup>&frasl;<sub>2</sub>&#8971;  // index of median
02 d = Select(A, m)  // dummy value, <em>semi-sorts</em> to get small values below median
03 d = Select(A, m-k-1)  // dummy value, <em>semi-sorts</em> to get m-k values below m-k
04 return A[m-k...m-1]
</code></pre>  

#### Java for k Largest Less Than Median  
```
[code here]
```  

## Dynamic Programming Algorithms  
[General Tips for Dynamic Programming](#general-tips-for-dynamic-programming)  
[Knapsack Problem](#knapsack-problem)  
[Longest Common Subsequence](#longest-common-subsequence)  
[Longest Common Substring](#longest-common-substring)  

### General Tips for Dynamic Programming
* try to identify sequence of steps -> __focus on last/incremental step__
* use previous steps/calculations to arrive at specific point
* breakdown information:
  1. subproblem: in words
  2. recurrence: equation -> __ALWAYS SOLVE BASE CASES__
* assume you are ending or doing something at A[k]
* consider transforming data and applying known algorithm solution
* for __contiguous subsequence__ problems examine __subproblems that end at i/j__

### Knapsack Problem  
__Input:__  
  * w<sub>1</sub>,...,w<sub>n</sub> weight of each item  
  * v<sub>1</sub>,...,v<sub>n</sub> value of each item  
  * W max weight limit  

__Output:__ max possible value that fits into backpack within constraint W  
__Idea:__  i<sup>th</sup> object has 2 possibilities:  
  1. object exceeds weight limit -> use max value of objects before i<sup>th</sup> object considered  
  2. object fits under weight limit:  
      * more value __with__ object -> add i<sup>th</sup> value to max value of i-1 objects at weight limit of w<sub>j</sub>-w<sub>i</sub>  
      * more value __without__ obect -> use max value of i-1 objects at same weight limit  

__Subproblem:__  M[i,j] = max possible value using v[1...i] and weights w[1...i] where &#8721;W &#8804; j  
__Recurrence:__  

| cell | value |  
| --- | --- |  
| M[i, j] = | <b>if</b> w<sub>i</sub> &#8804; j, <b>then</b> max { v<sub>i</sub> + M[i-1, j-w[i]], M[i-1, j] } |  
|  | <b>else:</b> M[i-1, j] |  

__Base Case:__  
  * M[i, 0] = 0  _no items at weight limit 0_  
  * M[0, j] = 0  _no items if 0 items allowed_  

__Run-time:__  T(n, W) = O(n * W)  

#### Pseudocode for Knapsack Problem  
<pre><code>
<b>Max-Value-In-Knapsack(w<sub>1</sub>,...,w<sub>n</sub>,v<sub>1</sub>,...,v<sub>n</sub>, W)</b>
01 create table M[0...n, 0...W]
02 for i = 0 to n
03 	M[i,0] = 0
04 for j = 0 to W
05 	M[0,j] = 0
06 for i = 1 to n
07 	for j = 1 to W
08 		if w[i] &#8804; j
09 			M[i,j] = max{ v[i] + M[i-1, j-w[i]], M[i-1, j] }
10 		else
11 			M[i,j] = M[i-1, j]
12 return M[n,W]
</code></pre>  

#### Java for Knapsack Problem  
```
[code here]

```
__Additional Resources for Algo Name__  
* Wikipedia: https://en.wikipedia.org/wiki/Knapsack_problem  
* GeeksforGeeks: http://www.geeksforgeeks.org/knapsack-problem/  

### Longest Common Subsequence 
__Input:__  two strings X[1...m], Y[1...n]  
__Output:__ the __length__ of the longest common subsequence (same order of letters, not necessarily contiguous)  
__Idea:__  adding i<sup>th</sup> letter to compare X[1...i] and Y[1...j] has 2 possibilities:  
  1. if X[i] matches Y[j] then there is one more longer subsequence compared to X[1...i-1] and Y[1...j-1]  
  2. otherwise same length subsequence as either X[1...i-1], Y[1...j] or X[1...i], Y[1...j-1]  

__Subproblem:__  M[i,j] = length of longest possible subsequence for X[1...i] and Y[1...j]  
__Recurrence:__  

| cell | value |  
| --- | --- |  
| M[i, j] = | <b>if</b> X[i] == Y[j], <b>then</b> M[i-1, j-1] + 1    
|  | <b>else:</b> max{ M[i-1, j], M[i, j-1]}    

__Base Case:__  
  * M[i, 0] = 0  _can't make a subsequence with zero letters_   
  * M[0, j] = 0  _can't make a subsequence with zero letters_   

__Run-time:__  T(m, n) = O(m * n)  

#### Pseudocode for Longest Common Subsequence  
<pre><code>
<b>Longest-Common-Subsequence(X[1...m], Y[1...n])</b>
01 create table M[0...m, 0...n]
02 for i = 0 to m
03 	M[i,0] = 0
04 for j = 0 to n
05 	M[0,j] = 0
06 for i = 1 to m
07 	for j = 1 to n
08 		if X[i] == Y[j]
09 			M[i,j] = M[i-1, j-1] + 1
10 		else
11 			M[i,j] = max{ M[i-1, j], M[i, j-1]}
12 return M[m,n]
</code></pre>  

#### Java for Longest Common Subsequence    
```
[code here]

```
__Additional Resources for Algo Name__  
* Wikipedia: https://en.wikipedia.org/wiki/Longest_common_subsequence_problem   
* GeeksforGeeks: http://www.geeksforgeeks.org/longest-common-subsequence/   

### Longest Common Substring  
__Input:__  two strings X[1...m], Y[1...n]  
__Output:__ the __length__ of the longest common substring (contiguous letters in same order)  
__Idea:__  the longest common substring that __ENDS__ at X[i], Y[j] has 2 possibilities:  
  1. if X[i] matches Y[j] then the longest common substring that __ENDS__ at X[i], Y[j] is one more letter long than the substring at X[1...i-1], Y[1...j-1]  
  2. otherwise the longest common substring that __ENDS_ at X[i], Y[j] is zero  

__Subproblem:__  L[i,j] = max length of longest substring that __ENDS__ at X[i], Y[j]  
__Recurrence:__  

| cell | value |  
| --- | --- |  
| L[i, j] = | <b>if</b> X[i] == Y[j], <b>then</b> L[i-1, j-1] + 1  
|  | <b>else:</b> 0  

__Base Case:__  
  * L[i, 0] = 0  _can't make a substring with zero letters_   
  * L[0, j] = 0  _can't make a substring with zero letters_   

__Run-time:__  T(m, n) = O(m * n)  

#### Pseudocode for Longest Common Substring  
<pre><code>
<b>Longest-Common-Substring(X[1...m], Y[1...n])</b>
01 create table L[0...m, 0...n]
02 for i = 0 to m
03 	L[i,0] = 0
04 for j = 0 to n
05 	L[0,j] = 0
06 maxLength = 0
07 for i = 1 to m
08 	for j = 1 to n
09 		if X[i] == Y[j]
10 			L[i,j] = L[i-1, j-1] + 1
11 		else
12 			L[i,j] = 0
13		maxLength = max{ maxLength, L[i,j] }
14 return maxLength
</code></pre>  

#### Java for Longest Common Substring  
```
[code here]

```
__Additional Resources for Algo Name__  
* Wikipedia: https://en.wikipedia.org/wiki/Longest_common_substring_problem  
* GeeksforGeeks: http://www.geeksforgeeks.org/longest-common-substring/  

