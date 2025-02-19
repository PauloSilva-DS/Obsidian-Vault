---
tags:
  - dataScienceZero
  - estudo
  - python
Completo: false
Atualizado: 2025-02-19  15.15
Criado: 2025-02-12  16.41
---
[[0 -Data Science do Zero]]


### **Resumo e Explicação do Capítulo 4 – Álgebra Linear**

O capítulo trata de álgebra linear, uma área da matemática que lida com espaços vetoriais e operações sobre vetores. Embora o autor reconheça que um capítulo não pode ensinar álgebra linear completamente, ele destaca a importância dessa disciplina em ciência de dados e introduz conceitos essenciais.

#### **Vetores**

Vetores são objetos que podem ser somados e multiplicados por escalares (números), formando novos vetores. Em termos concretos, eles representam pontos em um espaço de dimensões finitas. Por exemplo:

- A altura, peso e idade de uma pessoa podem ser representados como um vetor tridimensional **[altura, peso, idade]**.
- Notas de provas podem ser representadas como um vetor de quatro dimensões **[nota1, nota2, nota3, nota4]**.

Em Python, vetores podem ser implementados como listas de números:

```run-python
from typing import List
Vector = List[float]
```

#### **Operações com Vetores**

Como listas comuns do Python não suportam operações matemáticas de vetores, é necessário implementar funções para essas operações:

1. **Soma de vetores:** soma os elementos correspondentes de dois vetores:
    
    ```run-python
    def add(v: Vector, w: Vector) -> Vector:
		assert len(v) == len(w), "vetores devem ter o mesmo tamanho"
	    return [v_i + w_i for v_i, w_i in zip(v, w)]
    ```
    
    Exemplo: `add([1, 2, 3], [4, 5, 6])` resulta em `[5, 7, 9]`.
    
2. **Subtração de vetores:** subtrai os elementos correspondentes:
    
    ```python
    def subtract(v: Vector, w: Vector) -> Vector:
        assert len(v) == len(w), "vetores devem ter o mesmo tamanho"
        return [v_i - w_i for v_i, w_i in zip(v, w)]
    ```
    
3. **Soma de uma lista de vetores:** calcula a soma de cada elemento correspondente em vários vetores:
    
    ```python
    def vector_sum(vectors: List[Vector]) -> Vector:
        assert vectors, "nenhum vetor fornecido!"
        num_elements = len(vectors[0])
        assert all(len(v) == num_elements for v in vectors), "tamanhos diferentes!"
        return [sum(vector[i] for vector in vectors) for i in range(num_elements)]
    ```
    
4. **Multiplicação por um escalar:** multiplica cada elemento do vetor por um número:
    
    ```python
    def scalar_multiply(c: float, v: Vector) -> Vector:
        return [c * v_i for v_i in v]
    ```
    
5. **Média de vetores:** calcula a média de uma lista de vetores:
    
    ```python
    def vector_mean(vectors: List[Vector]) -> Vector:
        n = len(vectors)
        return scalar_multiply(1/n, vector_sum(vectors))
    ```
    

#### **Produto Escalar (Dot Product)**

O **produto escalar** entre dois vetores é a soma dos produtos de seus elementos correspondentes:

```python
def dot(v: Vector, w: Vector) -> float:
    assert len(v) == len(w), "vetores devem ter o mesmo tamanho"
    return sum(v_i * w_i for v_i, w_i in zip(v, w))
```

Se `w` for um vetor unitário (magnitude 1), o produto escalar mede o quanto `v` se estende na direção de `w`.

#### **Magnitude e Distância de Vetores**

A **magnitude** de um vetor é dada pela raiz quadrada da soma de seus elementos ao quadrado (norma Euclidiana):

```python
import math
def magnitude(v: Vector) -> float:
    return math.sqrt(sum(v_i ** 2 for v_i in v))
```

A **distância** entre dois vetores pode ser calculada subtraindo um vetor do outro e obtendo sua magnitude:

```python
def distance(v: Vector, w: Vector) -> float:
    return magnitude(subtract(v, w))
```

#### **Conclusão**

Essas operações são fundamentais para aplicações em ciência de dados e aprendizado de máquina. Embora listas do Python sejam úteis para aprendizado, bibliotecas como **NumPy** oferecem implementações mais eficientes para uso em produção.











