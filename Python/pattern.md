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
