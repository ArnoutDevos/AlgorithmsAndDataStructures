# Sorting
Implementations of sorting algorithms.

## Sorting in Python
```python
>>> sorted([5, 2, 3, 1, 4])
[1, 2, 3, 4, 5]
>>> a = [5, 2, 3, 1, 4]
>>> a.sort()
>>> a
[1, 2, 3, 4, 5]
```
## Insertion sort
```python
def insertionSort(alist):
   for index in range(1,len(alist)):

     currentvalue = alist[index]
     position = index

     while position>0 and alist[position-1]>currentvalue:
         alist[position]=alist[position-1]
         position = position-1

     alist[position]=currentvalue
```
## Quicksort
```python
def quicksort(x):
    if len(x) == 1 or len(x) == 0:
        return x
    else:
        pivot = x[0]
        i = 0
        for j in range(len(x)-1):
            if x[j+1] < pivot:
                x[j+1],x[i+1] = x[i+1], x[j+1]
                i += 1
        x[0],x[i] = x[i],x[0]
        first_part = quicksort(x[:i])
        second_part = quicksort(x[i+1:])
        first_part.append(x[i])
        return first_part + second_part
```
## Mergesort
### Bigger function
```python
def mergesort(x):
    if len(x) == 0 or len(x) == 1:
        return x
    else:
        middle = len(x)/2
        a = mergesort(x[:middle])
        b = mergesort(x[middle:])
        return merge(a,b)
```
### Merge subroutine
```python
def merge(a,b):
    c = []
    while len(a) != 0 and len(b) != 0:
        if a[0] < b[0]:
            c.append(a[0])
            a.remove(a[0])
        else:
            c.append(b[0])
            b.remove(b[0])
    if len(a) == 0:
        c += b
    else:
        c += a
    return c
```
## Mergesort better than Quicksort?
In general **Quicksort** is **better**:
- Better **locality of reference** than mergesort (faster accesses)
- Memory Worst-case `O(log(n))` (stack calls) <-> Mergesort `O(n)` memory for merging

**BUT**, for some cases **Mergesort** is **better**:
1. MergeSort is **stable** by design, equal elements keep their **original order**.
2. Easily implemented **parallel** (multithreading)
3. **Guaranteed worst-case** of `O(nlog(n))`