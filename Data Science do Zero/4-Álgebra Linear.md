---
tags:
  - dataScienceZero
  - estudo
  - python
Completo: false
Atualizado: 2025-02-19  15.34
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

### **Produto Escalar (Dot Product)**

O **produto escalar** (ou **produto interno**) é uma operação entre dois vetores que retorna um número (escalar). Ele é calculado somando os produtos dos elementos correspondentes dos vetores.

#### **Fórmula do Produto Escalar**

Se temos dois vetores vv e ww de mesmo tamanho nn:

v=[v1,v2,...,vn]v = [v_1, v_2, ..., v_n] w=[w1,w2,...,wn]w = [w_1, w_2, ..., w_n]

O produto escalar é definido como:

v⋅w=v1⋅w1+v2⋅w2+...+vn⋅wnv \cdot w = v_1 \cdot w_1 + v_2 \cdot w_2 + ... + v_n \cdot w_n

#### **Exemplo**

Se temos os vetores:

v=[1,2,3],w=[4,5,6]v = [1, 2, 3], \quad w = [4, 5, 6]

O produto escalar será:

1×4+2×5+3×6=4+10+18=321 \times 4 + 2 \times 5 + 3 \times 6 = 4 + 10 + 18 = 32

Em **Python**, podemos implementar isso assim:

```python
def dot(v: list, w: list) -> float:
    assert len(v) == len(w), "Os vetores devem ter o mesmo tamanho"
    return sum(v_i * w_i for v_i, w_i in zip(v, w))

# Testando
print(dot([1, 2, 3], [4, 5, 6]))  # Saída: 32
```

---

### **Interpretação Geométrica**

O produto escalar também pode ser definido em termos do **ângulo** θ\theta entre os vetores:

v⋅w=∣v∣⋅∣w∣⋅cos⁡(θ)v \cdot w = |v| \cdot |w| \cdot \cos(\theta)

Onde:

- ∣v∣|v| e ∣w∣|w| são as **magnitudes** (ou normas) dos vetores, calculadas como: ∣v∣=v12+v22+...+vn2|v| = \sqrt{v_1^2 + v_2^2 + ... + v_n^2}
- cos⁡(θ)\cos(\theta) é o cosseno do ângulo entre os vetores.

#### **Relação com o Ângulo**

- Se v⋅w>0v \cdot w > 0, os vetores formam um ângulo **menor que 90º** (apontam em direções semelhantes).
- Se v⋅w<0v \cdot w < 0, os vetores formam um ângulo **maior que 90º** (apontam em direções opostas).
- Se v⋅w=0v \cdot w = 0, os vetores são **ortogonais** (ângulo de 90º, ou seja, são perpendiculares).

#### **Exemplo de Ângulo entre Vetores**

Podemos calcular o ângulo entre dois vetores rearranjando a fórmula:

θ=cos⁡−1(v⋅w∣v∣∣w∣)\theta = \cos^{-1} \left( \frac{v \cdot w}{|v| |w|} \right)

Em Python:

```python
import math

def magnitude(v):
    return math.sqrt(sum(v_i ** 2 for v_i in v))

def angle_between(v, w):
    return math.acos(dot(v, w) / (magnitude(v) * magnitude(w)))

# Testando
v = [1, 0]
w = [0, 1]
print(math.degrees(angle_between(v, w)))  # Saída: 90.0 (vetores ortogonais)
```

---

### **Aplicações do Produto Escalar**

1. **Ciência de Dados e Machine Learning** – Similaridade entre vetores de características (exemplo: recomendação de produtos).
2. **Gráficos Computacionais** – Iluminação e renderização em computação gráfica.
3. **Física** – Trabalho realizado por uma força é dado pelo produto escalar entre a força e o deslocamento.

---

O **produto escalar** é uma ferramenta essencial na álgebra linear, pois relaciona diretamente as magnitudes dos vetores e o ângulo entre eles! 🚀


## **Magnitude e Distância entre Vetores**

Esses dois conceitos vêm da álgebra linear e são fundamentais para entender o espaço vetorial. Vamos explorar cada um separadamente.

---

## **Magnitude de um Vetor (Norma ou Comprimento)**

A **magnitude** de um vetor (também chamada de **norma** ou **comprimento**) representa o "tamanho" do vetor no espaço. É calculada usando a fórmula da distância Euclidiana a partir da origem até o ponto representado pelo vetor.

### **Fórmula da Magnitude**

Se temos um vetor vv em um espaço nn-dimensional:

v=[v1,v2,...,vn]v = [v_1, v_2, ..., v_n]

A magnitude de vv, denotada por ∣v∣|v| ou ∣∣v∣∣||v||, é:

∣v∣=v12+v22+...+vn2|v| = \sqrt{v_1^2 + v_2^2 + ... + v_n^2}

Isso vem diretamente do Teorema de Pitágoras!

### **Exemplo**

Se tivermos um vetor em 2D:

v=[3,4]v = [3, 4]

A magnitude será:

∣v∣=32+42=9+16=25=5|v| = \sqrt{3^2 + 4^2} = \sqrt{9 + 16} = \sqrt{25} = 5

### **Implementação em Python**

```python
import math

def magnitude(v):
    """Calcula a magnitude (norma) de um vetor"""
    return math.sqrt(sum(v_i ** 2 for v_i in v))

# Testando
print(magnitude([3, 4]))  # Saída: 5.0
```

### **Interpretação Geométrica**

A magnitude representa o comprimento do vetor no espaço. Em um gráfico, se desenharmos um vetor [3,4][3,4] a partir da origem, ele formará um triângulo retângulo com catetos 3 e 4, e a hipotenusa será 5.

---

## **Distância entre Dois Vetores**

A **distância entre dois vetores** mede o quão "longe" eles estão um do outro no espaço vetorial. A forma mais comum de medir essa distância é usando a **distância Euclidiana**.

### **Fórmula da Distância Euclidiana**

Se temos dois vetores vv e ww:

v=[v1,v2,...,vn]v = [v_1, v_2, ..., v_n] w=[w1,w2,...,wn]w = [w_1, w_2, ..., w_n]

A distância entre eles é:

d(v,w)=(v1−w1)2+(v2−w2)2+...+(vn−wn)2d(v, w) = \sqrt{(v_1 - w_1)^2 + (v_2 - w_2)^2 + ... + (v_n - w_n)^2}

Ou seja, é a magnitude do vetor obtido ao subtrair ww de vv.

### **Exemplo**

Se tivermos os vetores:

v=[1,2,3],w=[4,6,8]v = [1, 2, 3], \quad w = [4, 6, 8]

A distância será:

d=(1−4)2+(2−6)2+(3−8)2d = \sqrt{(1 - 4)^2 + (2 - 6)^2 + (3 - 8)^2} d=(−3)2+(−4)2+(−5)2d = \sqrt{(-3)^2 + (-4)^2 + (-5)^2} d=9+16+25=50≈7.07d = \sqrt{9 + 16 + 25} = \sqrt{50} \approx 7.07

### **Implementação em Python**

```python
def distance(v, w):
    """Calcula a distância Euclidiana entre dois vetores"""
    return math.sqrt(sum((v_i - w_i) ** 2 for v_i, w_i in zip(v, w)))

# Testando
print(distance([1, 2, 3], [4, 6, 8]))  # Saída: 7.07
```

### **Interpretação Geométrica**

Se tivermos dois pontos A(1,2,3)A(1,2,3) e B(4,6,8)B(4,6,8), a distância entre eles no espaço tridimensional é a hipotenusa do triângulo formado pelos deslocamentos em cada dimensão.

---

## **Resumo**

1. **Magnitude** de um vetor representa seu comprimento e é calculada como ∑vi2\sqrt{\sum v_i^2}.
2. **Distância** entre dois vetores mede quão longe eles estão um do outro e é calculada como ∑(vi−wi)2\sqrt{\sum (v_i - w_i)^2}.
3. **Ambas** as fórmulas vêm do Teorema de Pitágoras e são muito usadas em computação gráfica, machine learning e análise de dados.

🚀 Agora você pode medir tamanhos de vetores e distâncias entre pontos no espaço!







