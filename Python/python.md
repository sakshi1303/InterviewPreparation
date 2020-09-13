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
