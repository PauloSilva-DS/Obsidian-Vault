---
Atualizado: 2025-03-02  10.11
Criado: 2025-02-26  14.26
---
```python
def fibonacci(n, k):
    if n == 0:
        return 0
    elif n == 1:
        return 1
    else:
        return fibonacci(n - 1, k) + (fibonacci(n - 1, 1) if k == 0 else 0)
```


```python
def fibonacci(n, k):
    if n == 0:
        return 0
    elif n == 1:
        return 1
    else:
        return fibonacci(n - 1, k) + (fibonacci(n - 1, 1) if k == 0 else 0)

for i in range(20):
	print(f"Term {i}:", fibonacci(i,i))
        
```
