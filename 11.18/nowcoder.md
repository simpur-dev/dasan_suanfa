https://www.nowcoder.com/practice/b1b9a88d8e3946e993232e2525f6a11c

```python
import sys

T = int(input())

for i in range(T):
    num = int(input())

    yes = False
    no = False

    for c in str(num):
        d = int(c)

        if d == 0 or num % d == 0:
            yes = True
        else:
            no = True
        
    if yes and no:
        print("H")
    elif no:
        print("S")
    else:
        print("G")
```
