## List 

#### Example 1
```python
my_list = []
print(my_list)

my_list = [1, 2, 3]
print(my_list)

my_list = [1, "Hello", 3.4]
print(my_list)

my_list = ["mouse", [8,4,6], ['a']]
print(my_list)
```
<details>
<summary>Answer</summary>
[]<br/>
[1, 2, 3]<br/>
[1, 'Hello', 3.4]<br/>
['mouse', [8, 4, 6], ['a']]<br/>
</details>

#### Example 2
```python
my_list = ['p', 'r', 'o', 'b', 'e']
print(my_list[0])
print(my_list[2])
print(my_list[4])
print(my_list[-1])
print(my_list[-5])
```

<details>
<summary>Answer</summary>
p
o
e
e
p
</details>

#### Example 3
```python
n_list = ["Happy", [2, 0, 1, 5]]
print(n_list[0][1])
print(n_list[1][3])
##print(n_list[4.0])  -- Error
```
<details>
<summary>Answer</summary>
p
a
5
</details>   

#### Example 4
```python
my_list = ['p','r','o','g','r','a','m','i','z']
print(my_list[2:5])
print(my_list[:-5])
print(my_list[5:])
print(my_list[:])
```
<details>
<summary>Answer</summary>
['o', 'g', 'r']<br/>
['p', 'r', 'o', 'g']<br/>
['a', 'm', 'i', 'z']<br/>
['p', 'r', 'o', 'g', 'r', 'a', 'm', 'i', 'z']<br/>
</details>  

#### Example 5
```python
odd = [2, 4, 6, 8]
odd[0] = 1
print(odd)

odd[1:4] = [3, 5, 7]
print(odd)

odd = [1, 3, 5]
odd.append(7)
print(odd)

odd.extend([9, 11, 13])
print(odd)

odd = [1, 3, 5]
print(odd + [9, 7, 5])

print(['re'] * 3)

odd = [1, 9]
odd.insert(1, 3)
print(odd)

odd[2:2] = [5, 7]
print(odd)
```

<details>
<summary>Answer</summary>
[1, 4, 6, 8]<br/>
[1, 3, 5, 7]<br/>
[1, 3, 5, 7]<br/>
[1, 3, 5, 7, 9, 11, 13]<br/>
[1, 3, 5, 9, 7, 5]<br/>
['re', 're', 're']<br/>
[1, 3, 9]<br/>
[1, 3, 5, 7, 9]<br/>
</details> 

#### Example 6

```python
my_list = ['p', 'r', 'o', 'b', 'l', 'e', 'm']
del my_list[2]
print(my_list)

del my_list[1:5]
print(my_list)

del my_list
##print(my_list)  -- Error

my_list = ['p','r','o','b','l','e','m']
my_list.remove('p')

print(my_list.pop(1))
print(my_list)

print(my_list.pop())
print(my_list)

my_list.clear()
print(my_list)

my_list = ['p','r','o','b','l','e','m']
my_list[2:3] = []

print(my_list)

my_list[2:5] = []
print(my_list)
```

<details>
<summary>Answer</summary>
['p', 'r', 'b', 'l', 'e', 'm']<br/>
['p', 'm']<br/>
o<br/>
['r', 'b', 'l', 'e', 'm']<br/>
m<br/>
['r', 'b', 'l', 'e']<br/>
[]<br/>
['p', 'r', 'b', 'l', 'e', 'm']<br/>
['p', 'r', 'm']<br/>
</details> 

#### Example 7

```python
my_list = [3, 8, 1, 6, 0, 8, 4]
print(my_list.index(8))
print(my_list.count(8))

my_list.sort()
print(my_list)

my_list.reverse()
print(my_list)

pow2 = [2 ** x for x in range(10)]
print(pow2)

pow2 = []
for x in range(10):
    pow2.append(2 ** x)
    
pow2 = [2 ** x for x in range(10) if x > 5]
print(pow2)    

odd = [x for x in range(20) if x%2 == 1]
print(odd)

a = [x+y for x in ['Python ', 'C '] for y in ['Language', 'Programming']]
print(a)

my_list = ['p', 'r', 'o', 'b', 'l', 'e', 'm']
print('p' in my_list)

print('a' in my_list)

print('c' not in my_list)

for fruit in ['apple', 'banana', 'mango']:
    print('I like', fruit)
```

<details>
<summary>Answer</summary>
1<br/>
2<br/>
[0, 1, 3, 4, 6, 8, 8]<br/>
[8, 8, 6, 4, 3, 1, 0]<br/>
[1, 2, 4, 8, 16, 32, 64, 128, 256, 512]<br/>
[64, 128, 256, 512]<br/>
[1, 3, 5, 7, 9, 11, 13, 15, 17, 19]<br/>
['Python Language', 'Python Programming', 'C Language', 'C Programming']<br/>
True<br/>
False<br/>
True<br/>
I like apple<br/>
I like banana<br/>
I like mango<br/>
</details> 

## Set

#### Example 1

```python
my_set = {1,2,3}
print(my_set)

my_set = {1.0,'Hello',(1,2,3)}
print(my_set)

my_set = {1,2,3,4,3,2}
print(my_set)

my_set = set([1,2,3,2])
print(my_set)

##my_set = {1,2,[3,4]}
```

<details>
<summary>Answer</summary>
{1, 2, 3}<br/>
{1.0, (1, 2, 3), 'Hello'}<br/>
{1, 2, 3, 4}<br/>
{1, 2, 3}<br/>
</details> 

#### Example 2

```python
a={}
print(type(a))

a=set()
print(type(a))

my_set = {1,3}
print(my_set)

my_set.add(2)
print(my_set)

my_set.update([2,3,4])
print(my_set)

my_set.update([4,5],{1,6,8})
print(my_set)
```

<details>
<summary>Answer</summary>
class 'dict'<br/>
class 'set'<br/>
{1, 3}<br/>
{1, 2, 3}<br/>
{1, 2, 3, 4}<br/>
{1, 2, 3, 4, 5, 6, 8}<br/>
</details> 

#### Example 3

```python
my_set = {1, 3, 4, 5, 6}
print(my_set)

my_set.discard(4)
print(my_set)

my_set.remove(6)
print(my_set)

my_set.discard(2)
print(my_set)

##my_set.remove(2)  --Error
##my_set.discard(2)

my_set = set("HelloWorld")
print(my_set)

my_set.pop()
print(my_set)

my_set.clear()
print(my_set)
```

<details>
<summary>Answer</summary>
{1, 3, 4, 5, 6}<br/>
{1, 3, 5, 6}<br/>
{1, 3, 5}<br/>
{1, 3, 5}<br/>
{'e', 'l', 'd', 'r', 'W', 'o', 'H'}<br/>
{'l', 'd', 'r', 'W', 'o', 'H'}<br/>
set()<br/>
</details>    

#### Example 4
```python
A = {1,2,3,4,5}
B = {4,5,6,7,8}

print(A | B)
print(A.union(B))

print(A & B)
print(A.intersection(B))

print(A - B)
print(A.difference(B))
print(B.difference(A))

print(A ^ B)
print(A.symmetric_difference(B))
print(B.symmetric_difference(A))
```

<details>
<summary>Answer</summary>
{1, 2, 3, 4, 5, 6, 7, 8}<br/>
{1, 2, 3, 4, 5, 6, 7, 8}<br/>
{4, 5}<br/>
{4, 5}<br/>
{1, 2, 3}<br/>
{1, 2, 3}<br/>
{8, 6, 7}<br/>
{1, 2, 3, 6, 7, 8}<br/>
{1, 2, 3, 6, 7, 8}<br/>
{1, 2, 3, 6, 7, 8}<br/>
</details>  

#### Example 5
```python
my_set = set("apple")
print('a' in my_set)
print('b' in my_set)
print('b' not in my_set)

for letter in set("apple"):
    print(letter)

A = frozenset([1,2,3,4])
B = frozenset([3,4,5,6])

print(A.isdisjoint(B))
print(A.difference(B))
print(A | B)

##print(A.add(3))
```

<details>
<summary>Answer</summary>
True<br/>
False<br/>
True<br/>
e<br/>
a<br/>
l<br/>
p<br/>
False<br/>
frozenset({1, 2})<br/>
frozenset({1, 2, 3, 4, 5, 6})<br/>
</details>  

## Tuple

#### Example 1

```python
my_tuple = ()
print(my_tuple)

my_tuple = (1,2,3)
print(my_tuple)

my_tuple = (1, "Hello", 3.4)
print(my_tuple)

my_tuple = ('mouse', [8,4,6], (1,2,3))
print(my_tuple)
```
<details>
<summary>Answer</summary>
()<br/>
(1, 2, 3)<br/>
(1, 'Hello', 3.4)<br/>
('mouse', [8, 4, 6], (1, 2, 3))<br/>
</details>    

#### Example 2

```python
## packing
my_tuple = 3, 4.6, 'dog'
print(my_tuple)

##unpacking
a,b,c = my_tuple
print(a)
print(b)
print(c)

my_tuple = ("hello")
print(type(my_tuple))

my_tuple = ("hello",)
print(type(my_tuple))

my_tuple = "hello",
print(type(my_tuple))
```
<details>
<summary>Answer</summary>
(3, 4.6, 'dog')<br/>
3<br/>
4.6<br/>
dog<br/>
class 'str'<br/>
class 'tuple'<br/>
class 'tuple'<br/>
</details>

#### Example 3
```python
my_tuple = ('p','e','r','m','i','t')
print(my_tuple[0])
print(my_tuple[5])

##print(my_tuple[6])
##print(my_tuple[2.0])

n_tuple = ("mouse", [8, 4, 6], (1, 2, 3))
print(n_tuple[0][3])
print(n_tuple[1][1])

my_tuple = ('p','e','r','m','i','t')
print(my_tuple[-1])
print(my_tuple[-6])
```
<details>
<summary>Answer</summary>
p
t
s
4
t
p
</details>   

#### Example 4
```python
my_tuple = ('p','r','o','g','r','a','m','i','z')
print(my_tuple[1:4])
print(my_tuple[:-7])
print(my_tuple[7:])
print(my_tuple[:])

my_tuple = (4, 2, 3, [6, 5])
##my_tuple[1] = 9

my_tuple[3][0] = 9
print(my_tuple)

my_tuple = ('p', 'r', 'o', 'g', 'r', 'a', 'm', 'i', 'z')
print(my_tuple)
```
<details>
<summary>Answer</summary>
('r', 'o', 'g')<br/>
('p', 'r')<br/>
('i', 'z')<br/>
('p', 'r', 'o', 'g', 'r', 'a', 'm', 'i', 'z')<br/>
(4, 2, 3, [9, 5])<br/>
('p', 'r', 'o', 'g', 'r', 'a', 'm', 'i', 'z')<br/>
</details>

#### Example 5
```python
print((1,2,3) + (4,5,6))
print(("Repeat",) * 3)

my_tuple = ('p', 'r', 'o', 'g', 'r', 'a', 'm', 'i', 'z')
##del my_tuple[3]

del my_tuple
##print(my_tuple)
```
<details>
<summary>Answer</summary>
(1, 2, 3, 4, 5, 6)<br/>
('Repeat', 'Repeat', 'Repeat')<br/>
</details>

#### Example 6
```python
my_tuple = ('a', 'p', 'p', 'l', 'e',)
print(my_tuple.count('p'))
print(my_tuple.index('l'))

my_tuple = ('a', 'p', 'p', 'l', 'e')

print('a' in my_tuple)
print('b' in my_tuple)
print('g' not in my_tuple)

for name in ('John', 'Kate'):
    print("Hello", name)
```

<details>
<summary>Answer</summary>
2<br/>
3<br/>
True<br/>
False<br/>
True<br/>
Hello John<br/>
Hello Kate<br/>
</details>

## Dictionary

#### Example 1
```python
my_dict = {}
print(my_dict)

my_dict = {1: 'apple', 2: 'ball'};
print(my_dict)

my_dict = {'name': 'John', 1:[2,4,3]}
print(my_dict)

my_dict = dict({1:'apple', 2:'ball'})
print(my_dict)

my_dict = dict([(1,'apple'), (2,'ball')])
print(my_dict)

my_dict = {'name':'Jack', 'age':26}
print(my_dict['name'])
print(my_dict.get('age'))

print(my_dict.get('address'))
##print(my_dict['address'])

my_dict = {'name': 'Jack', 'age': 26}
my_dict['age'] = 27
print(my_dict)

my_dict['address'] = 'Downtown'
print(my_dict)
```

<details>
<summary>Answer</summary>
{}<br/>
{1: 'apple', 2: 'ball'}<br/>
{'name': 'John', 1: [2, 4, 3]}<br/>
{1: 'apple', 2: 'ball'}<br/>
{1: 'apple', 2: 'ball'}<br/>
Jack<br/>
26<br/>
None<br/>
{'name': 'Jack', 'age': 27}<br/>
{'name': 'Jack', 'age': 27, 'address': 'Downtown'}<br/>
</details>

#### Example 2
```python
squares = {1: 1, 2: 4, 3: 9, 4: 16, 5: 25}
print(squares.pop(4))
print(squares)

print(squares.popitem())
print(squares)

squares.clear()
print(squares)

del squares
##print(squares)
```

<details>
<summary>Answer</summary>
16<br/>
{1: 1, 2: 4, 3: 9, 5: 25}<br/>
(5, 25)<br/>
{1: 1, 2: 4, 3: 9}<br/>
{}<br/>
</details>

#### Example 3
```python
marks = {}.fromkeys(['Math','English','Science'],0)
print(marks)

for item in marks.items():
    print(item)

print(list(sorted(marks.keys())))
```

<details>
<summary>Answer</summary>
{'Math': 0, 'English': 0, 'Science': 0}<br/>
('Math', 0)<br/>
('English', 0)<br/>
('Science', 0)<br/>
['English', 'Math', 'Science']<br/>
</details>

#### Example 4
```python
squares = {x : x*x for x in range(6)}
print(squares)

squares = {}
for x in range(6):
    squares[x] = x*x
print(squares)

odd_squares  = {x : x*x for x in range(11) if x%2 == 1}
print(odd_squares)

squares = {1: 1, 3: 9, 5: 25, 7: 49, 9: 81}
print(1 in squares)
print(2 not in squares)
print(49 in squares)
```

<details>
<summary>Answer</summary>
{0: 0, 1: 1, 2: 4, 3: 9, 4: 16, 5: 25}<br/>
{0: 0, 1: 1, 2: 4, 3: 9, 4: 16, 5: 25}<br/>
{1: 1, 3: 9, 5: 25, 7: 49, 9: 81}<br/>
True<br/>
True<br/>
False<br/>
</details>

#### Example 5
```python
squares = {1: 1, 3: 9, 5: 25, 7: 49, 9: 81}
for i in squares:
    print(squares[i])
    
squares = {0: 0, 1: 1, 3: 9, 5: 25, 7: 49, 9: 81}
print(all(squares))
print(any(squares))
print(len(squares))
print(sorted(squares))
```

<details>
<summary>Answer</summary>
1<br/>
9<br/>
25<br/>
49<br/>
81<br/>
False<br/>
True<br/>
6<br/>
[0, 1, 3, 5, 7, 9]<br/>
</details>


## For & While Loop

#### Example 1

```python
num = [6, 5, 3, 8, 4, 2]
sum = 0
for val in num:
    sum = sum+val
print('The sum is =',sum)    
```

<details>
<summary>Answer</summary>
The sum is = 28
</details>

#### Example 2
```python
print(range(10))
print(list(range(10)))
print(list(range(2,8)))
print(list(range(2,20,3)))
```

<details>
<summary>Answer</summary>
range(0, 10)<br/>
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]<br/>
[2, 3, 4, 5, 6, 7]<br/>
[2, 5, 8, 11, 14, 17]<br/>
</details>

#### Example 3

```python
genre = ['pop', 'rock', 'jazz']

for i in range(len(genre)):
    print('I like', genre[i])
```

<details>
<summary>Answer</summary>
I like pop<br/>
I like rock<br/>
I like jazz<br/>
</details>

#### Example 4

```python
digits = [0,1,5]

for i in digits:
    print(i)
else:
    print('No items left')
```

<details>
<summary>Answer</summary>
0
1
5
No items left
</details>

#### Example 5

```python
n = 10
sum = 0
i = 1
while i <= n:
    sum = sum + i
    i = i + 1
print('The sum is', sum) 
```

<details>
<summary>Answer</summary>
The sum is 55
</details>

#### Example 6

```python
counter = 0
while counter < 3:
    print('Inside Loop')
    counter = counter + 1
else:
    print('Inside else')
```

<details>
<summary>Answer</summary>
Inside Loop<br/>
Inside Loop<br/>
Inside Loop<br/>
Inside else<br/>
</details>

## Pandas DataFrame

#### Example 1

```python
import pandas as pd
lst = ['Geeks','For','Geeks','is','portal','for','Geeks']
df = pd.DataFrame(lst)
print(df)
```
<details>
<summary>Answer</summary>
        0<br/>
0   Geeks<br/>
1     For<br/>
2   Geeks<br/>
3      is<br/>
4  portal<br/>
5     for<br/>
6   Geeks<br/>
</details>

#### Example 2

```python
import pandas as pd
data  = {'Name' : ['Tom', 'Nick', 'Krish', 'Jack'],
         'Age' : [20, 21, 19, 28]}
df = pd.DataFrame(data)
print(df)
```
<details>
<summary>Answer</summary>
    Name  Age<br/>
0    Tom   20<br/>
1   Nick   21<br/>
2  Krish   19<br/>
3   Jack   28<br/>    
</details>

#### Example 3

```python
import pandas as pd
data = pd.read_csv('emp.csv', index_col='emp_name', delimiter='|')
first = data.loc['John Doe']
second = data.loc['Tony Stark']
print(first, '\n\n\n', second)
```
<details>
<summary>Answer</summary>
emp_id                         1<br/>
emp_age                       30<br/>
emp_address               London<br/>
email          john.doe@mail.com<br/>
contact_no             987654321<br/>
dept                          HR<br/>
salary                     50000<br/>
Name: John Doe, dtype: object<br/>
<br/>
emp_id                           5<br/>
emp_age                         33<br/>
emp_address               New York<br/>
email          tony.stark@mail.com<br/>
contact_no              2345671267<br/>
dept                       FINANCE<br/>
salary                       45000<br/>
Name: Tony Stark, dtype: object<br/>
</details>

#### Example 4

```python
import pandas as pd
data = pd.read_csv('emp.csv', index_col='emp_name', delimiter='|')
row = data.iloc[3]
print(row)
```
<details>
<summary>Answer</summary>
emp_id                          4<br/>
emp_age                        29<br/>
emp_address                Canada<br/>
email          tom.gibbs@mail.com<br/>
contact_no             8769342104<br/>
dept                           IT<br/>
salary                      55000<br/>
Name: Tom Gibbs, dtype: object<br/>
</details>

#### Example 5

```python
import pandas as pd
import numpy as np
dict = {'First Score':[100,90,np.nan,95],
        'Second Score':[30,np.nan,45,56],
        'Third Score':[52,40,80,98],
        'Fourth Score':[np.nan,np.nan,np.nan,65]
        }
df = pd.DataFrame(dict)
df.fillna(0)
print(df)
df.dropna()
print(df)
```
<details>
<summary>Answer</summary>
   First Score  Second Score  Third Score  Fourth Score<br/>
0        100.0          30.0           52           0.0<br/>
1         90.0           0.0           40           0.0<br/>
2          0.0          45.0           80           0.0<br/>
3         95.0          56.0           98          65.0<br/>
   First Score  Second Score  Third Score  Fourth Score<br/>
3         95.0          56.0           98          65.0<br/>
</details>

## Merge Sort

```python
## Divide and Conquer 
## First divide the array into smaller subarrays recursively until subarray contains min no of element i.e. 1
## Now merge the subarrays to form a larger/combined sorted array

## Complexity :
    ## Time 
       ## Best  - O(nlogn)
       ## Avg   - O(nlogn)
       ## Worst - O(nlogn)
    ## Space - O(n) 

import math
def merge_sort(arr,p,r):
    if p < r:
        q = (p+r)//2
        merge_sort(arr,p,q)
        merge_sort(arr,q+1,r)
        merge(arr,p,q,r)
    
def merge(arr,p,q,r):
    L = arr[p:q+1]
    R = arr[q+1:r+1]
    L.append(math.inf)
    R.append(math.inf)
    i,j = 0,0
    for k in range(p,r+1):
        if L[i] <= R[j]:
            arr[k] = L[i]
            i = i+1
        else:
            arr[k] = R[j]
            j = j+1
        
arr = [48, 56, 92, 37, 11]
merge_sort(arr,0,len(arr)-1)
print('Sorted Array:', arr)
```
<details>
<summary>Answer</summary>
Sorted Array: [11, 37, 48, 56, 92]
</details>

## Heap Sort

```python
## First Step is to build a Max Heapify using first loop in heapSort function
## that calls heapify function. 

## Second loop is to sort the Max-Heap build in the first step which includes:
    ## 1. Swap
    ## 2. Remove
    ## 3. Heapify
    
## Complexity :
    ## Time - nlogn
    ## Space - O(1)    


def heapSort(arr):
    n = len(arr)
    ## Heapify
    for i in range(n, -1, -1):
        heapify(arr, n, i)    
    ## Now apply Heap Sort    
    for i in range(n-1, 0, -1):
        arr[i], arr[0] = arr[0], arr[i]
        heapify(arr, i, 0)

##Max Heapify        
def heapify(arr, n, i) :  
    largest = i;
    l = 2*i + 1
    r = 2*i + 2
    if l < n and arr[largest] < arr[l]:
        largest = l
    if r < n and arr[largest] < arr[r]:
        largest = r
    if largest != i:
        arr[i], arr[largest] = arr[largest], arr[i]
        heapify(arr, n, largest)
        
arr = [42, 76, 81, 39, 16]      
heapSort(arr) 
print('Sorted Array:', arr)
```
<details>
<summary>Answer</summary>
Sorted Array: [16, 39, 42, 76, 81]
</details>

## Binary Search 

#### Recursive Approach

```python
## Divide an Conquer to find element at middle position of array
## First Sort the elements if not sorted 
## Two ways of implementation 
  ## 1. Iterative Method
  ## 2. Recursive Method - We are using this approach that uses divide and conquer algorithm
  
## Complexity :
    ## Time 
       ## Best  - O(1)
       ## Avg   - O(logn)
       ## Worst - O(logn)
    ## Space - O(n)    

def binary_search(arr,low,high,key):
    if low <= high:
        mid = round((low+high)/2)
        if key == arr[mid]:
            return mid
        elif key > arr[mid]:
            low = mid + 1
            return binary_search(arr,low,high,key)
        elif key < arr[mid]:
            high = mid - 1
            return binary_search(arr,low,high,key)   
    else:
        return -1
        
        
arr = [3,4,5,6,7,8,9]
print(arr)
print('Enter the key:')
key = int(input())
low = 0
high = len(arr)-1
res = binary_search(arr,low,high,key)
if res != -1:
    print('Key is found at location:', res) 
else:
    print('Key not found')
```

<details>
<summary>Answer</summary>
[3, 4, 5, 6, 7, 8, 9]<br/>
Enter the key:<br/>
5<br/>
Key is found at location: 2<br/>
</details>

#### Iterative Approach

```python
## To find element at middle position of array
## First Sort the elements if not sorted 
## Two ways of implementation 
  ## 1. Iterative Method - We are using this approach
  ## 2. Recursive Method 
  
## Complexity :
    ## Time 
       ## Best  - O(1)
       ## Avg   - O(logn)
       ## Worst - O(logn)
    ## Space - O(n)    

def binary_search(arr,low,high,key):
    while low <= high:
        mid = round((low+high)/2)
        if key == arr[mid]:
            return mid
        elif key > arr[mid]:
            low = mid + 1
        elif key < arr[mid]:
            high = mid - 1
    else:
        return -1
        
arr = [3,4,5,6,7,8,9]
print(arr)
print('Enter the key:')
key = int(input())
low = 0
high = len(arr)-1
res = binary_search(arr,low,high,key)
if res != -1:
    print('Key is found at location:', res) 
else:
    print('Key not found')
```

<details>
<summary>Answer</summary>
[3, 4, 5, 6, 7, 8, 9]<br/>
Enter the key:<br/>
0<br/>
Key not found<br/>
</details>

## Searching a file in filesystem

#### Find all files in a directory with last name as .pyc (in python and UNIX)

<details>
<summary>Answer</summary>

```python
import os
 for path, dirs, files in os.walk('.'):
     for file in files :
         if file.endswith('.pyc'):
             print(file)
```
```unix
find . -name "*.pyc"
```
</details>

## Palindrome

#### Find minimum number of changes in a string to convert it into Palindrome

<details>
<summary>Answer</summary>
    
```python
def palindrome(string): 
    cnt_steps=0 
    for i in range(len(string)//2): 
        if string[i] != string[(i+1)*-1]: 
            cnt_steps=cnt_steps + 1 
    return cnt_steps 
res = palindrome('00001100')
print(res)
```
</details>

## Multithreading

#### https://github.com/sakshi1303/Python/blob/master/MultiThreading/MultiThreading.md

```python
from threading import Thread
from queue import Queue

## Main Function

p_start_date1 = '01-JAN-2003'
p_end_date1   = '31-DEC-2003'
p_start_date2 = '01-JAN-2004'
p_end_date2   = '31-DEC-2004'
p_start_date3 = '01-JAN-2005'
p_end_date3   = '31-DEC-2005'
p_start_date4 = '01-JAN-2006'
p_end_date4   = '31-DEC-2006'
p_start_date5 = '01-JAN-2007'
p_end_date5   = '31-DEC-2007'
result_list = [[p_start_date1, p_end_date1], [p_start_date2, p_end_date2],
               [p_start_date3, p_end_date3], [p_start_date4, p_end_date4],
               [p_start_date5, p_end_date5]]
queue = Queue()
for elem in result_list:
    queue.put(elem)
thread_list = []    
for i in range(3):
    thread = Consumer(queue)
    thread_list.append(thread)
    thread_list[i].start()
for thread in thread_list:
    thread.join()

## Consumer Class

class Consumer(Thread) :
    def __init__(self, queue):
        Thread.__init__(self)
        self.queue = queue
    
    def run(self):
        while not self.queue.empty():
            elem = self.queue.get()
            p_start_date = str(elem[0])
            p_end_date   = str(elem[1])
            print('Start time:' + str(datetime.datetime.now()) + ' ' + p_start_date + ' ' + p_end_date)
            qry = """ CREATE TABLE TEST_HISTORY_""" + str(p_start_date[-4:0]) + """ AS 
                      SELECT t.*, RANK() OVER (PARTITION BY TEST_DATE ORDER BY TEST_NAME) AS RN
                      FROM TEST
                      WHERE TEST_DATE BETWEEN TO_DATE('{p_start_date}', 'DD-MON-YYYY') AND TO_DATE('{p_end_date}', 'DD-MON-YYYY') 
                      """.format(p_start_date=p_start_date, p_end_date=p_end_date)
            conn = cx.connect(username, pwd, host) 
            cursor = conn.cursor()
            cursor.execute(qry)
            print('Start time:' + str(datetime.datetime.now()) + ' ' + p_start_date + ' ' + p_end_date)
            conn.close()
            self.queue.task_done()
```

## Multiprocessing

#### Example 1

```python
import multiprocessing as mp

def foo(q):
    q.put('hello')
    
if __name__ == '__main__':
    mp.get_start_method('spawn')
    q = mp.Queue()
    p = mp.Process(target=foo, args=(q,))
    p.start()
    print(q.get())
    p.join()
    
```

#### Example 2

```python
import multiprocessing as mp

def foo(q):
    q.put('hello')
    
if __name__ == '__main__':
    ctx = mp.get_context('spawn')
    q = ctx.Queue()
    p = ctx.Process(target=foo, args=(q,))
    p.start()
    print(q.get())
    p.join()

```    
## Pool

#### Example 1

```python
from multiprocessing import Pool

def f(x):
    return x*x

if __name__ == '__main__':
    with Pool(5) as p:
        print(p.map(f, [1,2,3]))

```
#### Example 2

```python
from multiprocessing import Process
import os

def info(title):
    print(title)
    print('module name:', __name__)
    print('parent process', os.getppid)
    print('process id:', os.getpid())

def f(name):
    info('function f')
    print('hello', name)

if __name__ == '__main__':
    info('main line')
    p = Process(target=f, args=('bob',))
    p.start()
    p.join()
    
```
