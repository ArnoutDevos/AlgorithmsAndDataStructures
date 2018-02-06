# Heaps
## Min-Heap in Python:
```python
# Python code to demonstrate working of 
# heapify(), heappush() and heappop()
 
# importing "heapq" to implement heap queue
import heapq
 
# initializing list
li = [5, 7, 9, 1, 3]
 
# using heapify to convert list into heap
heapq.heapify(li)
 
# printing created heap
print ("The created heap is : ",end="")
print (list(li))
 
# using heappush() to push elements into heap
# pushes 4
heapq.heappush(li,4)
 
# printing modified heap
print ("The modified heap after push is : ",end="")
print (list(li))
 
# using heappop() to pop smallest element
print ("The popped and smallest element is : ",end="")
print (heapq.heappop(li))
```
## Example: Median finding of stream
The median of a dataset of integers is the midpoint value of the dataset for which an equal number of integers are less than and greater than the value. To find the median, you must first sort your dataset of integers in non-decreasing order.
### With two heaps
You need a min-heap and a max-heap.

Min-heap will contain the maximum half of the numbers from the array. Max-heap will contain the minimum half of the numbers from the array.

So the top value from the min-heap will be the minimum number from the max half of the array. The top value from the max-heap will be the maximum number from the min half of the array.

So you pretty much have the middle values of the array, therefore can calculate the median. So think about it, what can you change to make it work with odd number of values?
### Other solution with bisect
```python
import sys
import bisect

n = int(input().strip())
a = []
a_i = 0
for a_i in range(n):
    a_t = int(input().strip())
    
    bisect.insort_left(a, a_t)
    
    if len(a) % 2 == 0:
        median = (a[int(len(a)/2-1)]+a[int(len(a)/2)])/2
    else:
        median = a[int(len(a)/2)]
    print("%.1f" % median)
```