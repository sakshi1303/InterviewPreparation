## Pattern

```python
n = int(input())

for i in range(1, n+1):
    a = n-1
    b = n-1
    for j in range(n-i):
        print('--',end='')
    for k in range(i-1):
        print(chr(a+97)+'-',end='')
        a = a-1
    for m in range(i):
        if chr(a+97) == chr(n-1+97):
            print(chr(a+97),end='')
        else:
            print(chr(a+97)+'-',end='')
        a = a+1
    for l in range(n-i):
        print('--',end='')
    print()
for i in range(1, n):
    a = n-1
    for j in range(i):
        print('--',end='')
    for k in range(n-i-1):
        print(chr(a+97)+'-',end='')
        a = a-1
    for m in range(n-i):
        if chr(a+97) == chr(n-1+97):
            print(chr(a+97),end='')
        else:
            print(chr(a+97)+'-',end='') 
        a = a+1
    for l in range(i):
        print('--',end='')
    print()
```        

## Solution from Upgrad

```python
n = int(input())

# Write your code below

#first we will print first half of the problem
#and then run the same loops in reverse for second half 

#first half
#each line is divided into 4 parts
#first -------
#then alphabets in descending order and '-'
#then alphabets in ascending order and '-'
#then again ------
for i in range(n-1,-1,-1):
    #first print '--'
    for j in range(i):
        print(end="--")
    
    #print alphabet and '-' in reverse order
    for j in range(n-1,i,-1):
        print(chr(j+97),end="-")
    
    #print alphabets in ascending order in same line and '-'
    for j in range(i,n):
        #if last line then dont print '-'
        if j != n-1:
            print(chr(j+97),end="-")
        else:
            print(chr(j+97),end="")
    
    #print '--' again
    for j in range(2*i):
        print(end="-")
    
    #after every line go to a new line to start all over again    
    print()
    

#note how everything remains same from previous line
#except the iteration of i
#in first half iteration of i was from [n-1, n-2, n-3, ... 2, 1, 0]
#in this half iteration of i is from [1, 2, 3, n-1]
#this means everything printed in previous line will be printed again but in reverse 
#order and without last line or where i = 0
for i in range(1,n):
    
    for j in range(i):
        print(end="--")
    for j in range(n-1,i,-1):
        print(chr(j+97),end="-")
    for j in range(i,n):
        if j != n-1:
            print(chr(j+97),end="-")
        else:
            print(chr(j+97),end="")
    for j in range(2*i):
        print(end="-")
    
    print()
```    
