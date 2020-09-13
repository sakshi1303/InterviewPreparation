## List 

```python
my_list = []
print(my_list)
```
<details>
<summary>Answer</summary>
[]
</details>

```
my_list = [1, 2, 3]
print(my_list)

my_list = [1, "Hello", 3.4]
print(my_list)

my_list = ["mouse", [8,4,6], ['a']]
print(my_list)

my_list = ['p', 'r', 'o', 'b', 'e']
print(my_list[0])
print(my_list[2])
print(my_list[4])
print(my_list[-1])
print(my_list[-5])

n_list = ["Happy", [2, 0, 1, 5]]

print(n_list[0][1])
print(n_list[1][3])

##print(n_list[4.0])  -- Error


my_list = ['p','r','o','g','r','a','m','i','z']

print(my_list[2:5])
print(my_list[:-5])
print(my_list[5:])
print(my_list[:])


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

## Set

```python
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

## Tuple

```python
my_tuple = ()
print(my_tuple)

my_tuple = (1,2,3)
print(my_tuple)

my_tuple = (1, "Hello", 3.4)
print(my_tuple)

my_tuple = ('mouse', [8,4,6], (1,2,3))
print(my_tuple)

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


print((1,2,3) + (4,5,6))
print(("Repeat",) * 3)


my_tuple = ('p', 'r', 'o', 'g', 'r', 'a', 'm', 'i', 'z')
##del my_tuple[3]

del my_tuple
##print(my_tuple)


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

## Dictionary

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


squares = {1: 1, 2: 4, 3: 9, 4: 16, 5: 25}
print(squares.pop(4))
print(squares)

print(squares.popitem())
print(squares)

squares.clear()
print(squares)

del squares
##print(squares)

marks = {}.fromkeys(['Math','English','Science'],0)
print(marks)

for item in marks.items():
    print(item)

print(list(sorted(marks.keys())))


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
print(49in squares)

squares = {1: 1, 3: 9, 5: 25, 7: 49, 9: 81}
for i in squares:
    print(squares[i])

squares = {0: 0, 1: 1, 3: 9, 5: 25, 7: 49, 9: 81}
print(all(squares))
print(any(squares))
print(len(squares))
print(sorted(squares))

```

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
