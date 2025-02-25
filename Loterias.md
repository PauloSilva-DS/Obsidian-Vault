> [!Lotofacil] 
> 

```python
import random  # importar as funções do random
random.seed()  # inicializar o contador
sorteados = [10,11,22,23]  # lista de números sorteados

# realização dos sorteios
for i in range(12):
    n = 10
    while (n in sorteados):
        n = random.randint(1, 25)
    sorteados.append(n)

sorteados.sort()
print(len(sorteados))
print(sorteados)


```

> [!Milionária] 

```python
import random  # importar as funções do random

random.seed()  # inicializar o gerador de números aleatórios
qtd = 6  # quantidade de números a serem sorteados
volantes = 1  # quantidade de apostas (volantes)
trevos = 2  # quantidade de trevos a serem sorteados (mínimo 2)

# realização dos sorteios
for y in range(volantes):
    sorteados = set()  # usar um conjunto para garantir que não haja números repetidos
    
    # sorteio dos números principais (1 a 50)
    while len(sorteados) < qtd:
        n = random.randint(1, 50)
        sorteados.add(n)
    
    lista_numeros = sorted(sorteados)  # ordenar os números sorteados
    
    # sorteio dos trevos (1 a 6)
    trevos_sorteados = set()
    while len(trevos_sorteados) < trevos:
        t = random.randint(1, 6)
        trevos_sorteados.add(t)
    
    lista_trevos = sorted(trevos_sorteados)
    
    # exibir o resultado
    print(f"Números sorteados: {lista_numeros}")
    print(f"Trevos sorteados: {lista_trevos}")




```

```python
print(67)
```


# 🤑 hh

