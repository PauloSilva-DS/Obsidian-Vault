---
tags:
  - estudo
  - python
  - AprendizadoMaquina
  - álgebraLinear
Completo: false
Atualizado: 2025-04-07  14.18
Criado: 2025-03-11  15.38
---
🔖[[Aprendizado de máquina]]

---

**Matemática - Álgebra Linear**

A Álgebra Linear é o ramo da matemática que estuda espaços vetoriais e transformações lineares entre espaços vetoriais, como girar uma forma, escaloná-la (aumentar ou diminuir seu tamanho), transladá-la (ou seja, movê-la), etc.

O Aprendizado de Máquina depende fortemente da Álgebra Linear, por isso é essencial entender o que são vetores e matrizes, quais operações podem ser realizadas com eles e como eles podem ser úteis.


### ✔ **Vetores**

**Definição**

Um vetor é uma quantidade definida por uma magnitude e uma direção. Por exemplo, a velocidade de um foguete é um vetor tridimensional: sua magnitude é a velocidade do foguete, e sua direção é (esperançosamente) para cima. Um vetor pode ser representado por um array de números chamados escalares. Cada escalar corresponde à magnitude do vetor em relação a cada dimensão.

Por exemplo, digamos que o foguete está subindo em um ângulo leve: ele tem uma velocidade vertical de 5.000 m/s, e também uma leve velocidade em direção ao Leste de 10 m/s, e uma leve velocidade em direção ao Norte de 50 m/s. A velocidade do foguete pode ser representada pelo seguinte vetor:
![[Pasted image 20250311154341.png]]

**Nota:** por convenção, os vetores são geralmente apresentados na forma de colunas. Além disso, os nomes dos vetores são geralmente em letras minúsculas para distingui-los das matrizes (que discutiremos abaixo) e em negrito (quando possível) para distingui-los de valores escalares simples, como <font color="#000000">`metros_por_segundo = 5026`.</font>

Uma lista de N números também pode representar as coordenadas de um ponto em um espaço N-dimensional, por isso é bastante frequente representar vetores como pontos simples em vez de setas. Um vetor com 1 elemento pode ser representado como uma seta ou um ponto em um eixo, um vetor com 2 elementos é uma seta ou um ponto em um plano, um vetor com 3 elementos é uma seta ou um ponto no espaço, e um vetor com N elementos é uma seta ou um ponto em um espaço N-dimensional... o que a maioria das pessoas acha difícil de imaginar.

---

### **Propósito**

Os vetores têm muitos propósitos no Aprendizado de Máquina, principalmente para representar observações e previsões. Por exemplo, digamos que construímos um sistema de Aprendizado de Máquina para classificar vídeos em 3 categorias (bom, spam, clickbait) com base no que sabemos sobre eles. Para cada vídeo, teríamos um vetor representando o que sabemos sobre ele, como:

![[Pasted image 20250311154907.png]]

Esse vetor poderia representar um vídeo que dura 10,5 minutos, mas apenas 5,2% dos espectadores assistem por mais de um minuto, ele recebe 3,25 visualizações por dia em média e foi sinalizado 7 vezes como spam. Como você pode ver, cada eixo pode ter um significado diferente.

Com base nesse vetor, nosso sistema de Aprendizado de Máquina pode prever que há uma probabilidade de 80% de ser um vídeo spam, 18% de ser clickbait e 2% de ser um bom vídeo. Isso pode ser representado como o seguinte vetor:

![[Pasted image 20250311155314.png]]


---

### **✅ Vetores em Python**

Em Python, um vetor pode ser representado de várias maneiras, a mais simples sendo uma lista regular de números:

```python
[10.5, 5.2, 3.25, 7.0]
```


Como planejamos fazer muitos cálculos científicos, é muito melhor usar o `ndarray` do NumPy, que fornece muitas implementações convenientes e otimizadas de operações matemáticas essenciais em vetores (para mais detalhes sobre o NumPy, confira o tutorial do NumPy). Por exemplo:

```python
import numpy as np

video = np.array([10.5, 5.2, 3.25, 7.0])
print(video)
```

---

### **Tamanho de um vetor**

O tamanho de um vetor pode ser obtido usando o atributo `size`:

```python
video.size
```

O $i^{th}$ elemento (também chamado de entrada ou item) de um vetor $\textbf{v}$  é denotado por $\textbf{v}_i$..



Observe que os índices em matemática geralmente começam em 1, mas na programação eles geralmente começam em 0. Portanto, para acessar o terceiro elemento de `video` programaticamente, escreveríamos:

```python
video[2]  # 3º elemento
```

---

### **Plotando vetores**

Para plotar vetores, usaremos o `matplotlib`, então vamos começar importando-o (para detalhes sobre o `matplotlib`, confira o tutorial do `matplotlib`):

```python
import matplotlib.pyplot as plt
```

---

### **Vetores 2D**

Vamos criar alguns vetores 2D muito simples para plotar:

```python
u = np.array([2, 5])
v = np.array([3, 1])
```

Esses vetores têm 2 elementos cada, então podem ser facilmente representados graficamente em um gráfico 2D, por exemplo, como pontos:

```python
x_coords, y_coords = zip(u, v)
plt.scatter(x_coords, y_coords, color=["r","b"])
plt.axis([0, 9, 0, 6])
plt.grid()
plt.show()
```

Os vetores também podem ser representados como setas. Vamos criar uma pequena função de conveniência para desenhar setas bonitas:

```python
def plot_vector2d(vector2d, origin=[0, 0], **options):
    return plt.arrow(origin[0], origin[1], vector2d[0], vector2d[1],
                     head_width=0.2, head_length=0.3, length_includes_head=True,
                     **options)
```

Agora vamos desenhar os vetores `u` e `v` como setas:

```python
plot_vector2d(u, color="r")
plot_vector2d(v, color="b")
plt.axis([0, 9, 0, 6])
plt.grid()
plt.show()
```


---

### **Vetores 3D**

Plotar vetores 3D também é relativamente simples. Primeiro, vamos criar dois vetores 3D:

```python
a = np.array([1, 2, 8])
b = np.array([5, 6, 3])


```

Agora vamos plotá-los usando o `Axes3D` do `matplotlib`:


```python
import matplotlib.pyplot as plt
import numpy as np
subplot3d = plt.subplot(111, projection='3d')
x_coords, y_coords, z_coords = zip(a,b)
subplot3d.scatter(x_coords, y_coords, z_coords)
subplot3d.set_zlim3d([0, 9])
plt.savefig('/home/paulo/Obsidian Vault/Obsidian Vault/plot/plot3d.png')

```

![[plot3d.png|500]]







É um pouco difícil visualizar exatamente onde no espaço esses dois pontos estão, então vamos adicionar linhas verticais.

Vamos criar uma pequena função de conveniência para plotar uma lista de vetores 3D com linhas verticais anexadas:

```python
def plot_vectors3d(ax, vectors3d, z0, **options):
    for v in vectors3d:
        x, y, z = v
        ax.plot([x, x], [y, y], [z0, z], color="gray", linestyle='dotted', marker=".")
    x_coords, y_coords, z_coords = zip(*vectors3d)
    ax.scatter(x_coords, y_coords, z_coords, **options)

subplot3d = plt.subplot(111, projection='3d')
subplot3d.set_zlim([0, 9])
plot_vectors3d(subplot3d, [a, b], 0, color=("r","b"))
#plt.show()
plt.savefig('/home/paulo/Obsidian Vault/Obsidian Vault/plot/plot3d.png')
```







---

### **Norma**

A norma de um vetor $( \mathbf{u} )$, denotada por $( \|\mathbf{u}\| )$, é uma medida do comprimento (também conhecida como magnitude) de $( \mathbf{u} )$.

Existem várias normas possíveis, mas a mais comum (e a única que discutiremos aqui) é a norma Euclidiana, que é definida como:


$\|\mathbf{u}\| = \sqrt{\sum_{i} \mathbf{u}_{i}^{2}}$


Isso é a raiz quadrada da soma dos quadrados de todos os componentes de $( \mathbf{u} )$. Poderíamos implementar isso facilmente em Python puro, lembrando que $( \sqrt{x} = x^{\frac{1}{2}} )$:

```python
def norma_euclidiana(u):
    return (sum(x**2 for x in u))**0.5

# Exemplo de uso
vetor = [3, 4]
resultado = norma_euclidiana(vetor)
print(f"A norma do vetor {vetor} é {resultado}")

```




No entanto, é muito mais eficiente usar a função `norm` do NumPy, disponível no módulo `linalg` (Álgebra Linear):

```python
import numpy.linalg as LA
u = np.array([2, 5]) 
print(LA.norm(u))
```






Vamos plotar um pequeno diagrama para confirmar que o comprimento do vetor $( \mathbf{u} )$ é de fato $( \approx 5,4 )$:



```python
radius = LA.norm(u)
plt.gca().add_artist(plt.Circle((0,0), radius, color="#DDDDDD"))
plot_vector2d(u, color="red")
plt.axis([0, 8.7, 0, 6])
plt.gca().set_aspect("equal")
plt.grid()
plt.savefig('/home/paulo/Obsidian Vault/Obsidian Vault/plot/plot3d.png')
```

Parece correto!

---

### **Adição**

Vetores de mesmo tamanho podem ser somados. A adição é realizada _elemento por elemento_:

```python
print(" ", u)
print("+", v)
print("-"*10)
print(u + v)
```


Vamos ver como a adição de vetores se parece graficamente:

```python
plot_vector2d(u, color="r")
plot_vector2d(v, color="b")
plot_vector2d(v, origin=u, color="b", linestyle="dotted")
plot_vector2d(u, origin=v, color="r", linestyle="dotted")
plot_vector2d(u+v, color="g")
plt.axis([0, 9, 0, 7])
plt.gca().set_aspect("equal")
plt.text(0.7, 3, "u", color="r", fontsize=18)
plt.text(4, 3, "u", color="r", fontsize=18)
plt.text(1.8, 0.2, "v", color="b", fontsize=18)
plt.text(3.1, 5.6, "v", color="b", fontsize=18)
plt.text(2.4, 2.5, "u+v", color="g", fontsize=18)
plt.grid()
plt.show()
```

A adição de vetores é **comutativa**, o que significa que $( \mathbf{u} + \mathbf{v} = \mathbf{v} + \mathbf{u} )$. Você pode ver isso na imagem anterior: seguir$( \mathbf{u} )$ e depois $( \mathbf{v} )$ leva ao mesmo ponto que seguir $( \mathbf{v} )$ e depois $( \mathbf{u} )$.

A adição de vetores também é **associativa**, o que significa que $( \mathbf{u} + (\mathbf{v} + \mathbf{w}) = (\mathbf{u} + \mathbf{v}) + \mathbf{w} )$.

Se você tiver uma forma definida por um número de pontos (vetores) e adicionar um vetor $( \mathbf{v})$ a todos esses pontos, então toda a forma será deslocada por $( \mathbf{v} )$. Isso é chamado de translação geométrica:

```python
t1 = np.array([2, 0.25])
t2 = np.array([2.5, 3.5])
t3 = np.array([1, 2])

x_coords, y_coords = zip(t1, t2, t3, t1)
plt.plot(x_coords, y_coords, "c--", x_coords, y_coords, "co")

plot_vector2d(v, t1, color="r", linestyle=":")
plot_vector2d(v, t2, color="r", linestyle=":")
plot_vector2d(v, t3, color="r", linestyle=":")

t1b = t1 + v
t2b = t2 + v
t3b = t3 + v

x_coords_b, y_coords_b = zip(t1b, t2b, t3b, t1b)
plt.plot(x_coords_b, y_coords_b, "b--", x_coords_b, y_coords_b, "bo")

plt.text(4, 4.2, "v", color="r", fontsize=18)
plt.text(3, 2.3, "v", color="r", fontsize=18)
plt.text(3.5, 0.4, "v", color="r", fontsize=18)

plt.axis([0, 6, 0, 5])
plt.gca().set_aspect("equal")
plt.grid()
plt.show()
```

Finalmente, subtrair um vetor é como adicionar o vetor oposto.

---

### **Multiplicação por um escalar**

Vetores podem ser multiplicados por escalares. Todos os elementos do vetor são multiplicados por esse número, por exemplo:

```python
print("1.5 *", u, "=")
print(1.5 * u)
```

Graficamente, a multiplicação por escalar resulta na mudança da escala de uma figura, daí o nome _escalar_. A distância da origem (o ponto nas coordenadas iguais a zero) também é multiplicada pelo escalar. Por exemplo, vamos aumentar a escala por um fator de k = 2,5:

```python
k = 2.5
t1c = k * t1
t2c = k * t2
t3c = k * t3

plt.plot(x_coords, y_coords, "c--", x_coords, y_coords, "co")

plot_vector2d(t1, color="r")
plot_vector2d(t2, color="r")
plot_vector2d(t3, color="r")

x_coords_c, y_coords_c = zip(t1c, t2c, t3c, t1c)
plt.plot(x_coords_c, y_coords_c, "b-", x_coords_c, y_coords_c, "bo")

plot_vector2d(k * t1, color="b", linestyle=":")
plot_vector2d(k * t2, color="b", linestyle=":")
plot_vector2d(k * t3, color="b", linestyle=":")

plt.axis([0, 9, 0, 9])
plt.gca().set_aspect("equal")
plt.grid()
plt.show()
```

Como você pode imaginar, dividir um vetor por um escalar é equivalente a multiplicar pelo seu inverso multiplicativo (recíproco):

$$

\frac{\mathbf{u}}{\lambda} = \frac{1}{\lambda} \times \mathbf{u}
$$



A multiplicação por escalar é **comutativa**: $$( \lambda \times \mathbf{u} = \mathbf{u} \times \lambda).

$$

Também é **associativa**:$$ ( \lambda_1 \times (\lambda_2 \times \mathbf{u}) = (\lambda_1 \times \lambda_2) \times \mathbf{u} ).
$$

Finalmente, é **distributiva** sobre a adição de vetores:
$$
( \lambda \times (\mathbf{u} + \mathbf{v}) = \lambda \times \mathbf{u} + \lambda \times \mathbf{v} ).
$$

---

### **Vetores nulos, unitários e normalizados**

- Um **vetor nulo** é um vetor cheio de 0s.
- Um **vetor unitário** é um vetor com norma igual a 1.
- O **vetor normalizado** de um vetor não nulo $( \mathbf{v}),$ denotado por $( \hat{\mathbf{v}} )$, é o vetor unitário que aponta na mesma direção que $( \mathbf{v}).$ Ele é igual a: $( \hat{\mathbf{v}} = \frac{\mathbf{v}}{\|\mathbf{v}\|}).$
-

```python
plt.gca().add_artist(plt.Circle((0, 0), 1, color='c'))
plt.plot(0, 0, "ko")
plot_vector2d(v / LA.norm(v), color="k", zorder=10)
plot_vector2d(v, color="b", linestyle=":", zorder=15)
plt.text(0.3, 0.3, r"$\hat{v}$", color="k", fontsize=18)
plt.text(1.5, 0.7, "$v$", color="b", fontsize=18)
plt.axis([-1.5, 5.5, -1.5, 3.5])
plt.gca().set_aspect("equal")
plt.grid()
plt.show()
```

---

### **Produto Escalar**

**Definição**

O produto escalar (também chamado de _produto interno_ no contexto do espaço Euclidiano) de dois vetores ( $\mathbf{u}$ ) e $( \mathbf{v} )$ é uma operação útil que aparece frequentemente em álgebra linear. Ele é denotado por $( \mathbf{u} \cdot \mathbf{v} )$, ou às vezes $( \langle \mathbf{u}|\mathbf{v} \rangle )$ ou $( (\mathbf{u}|\mathbf{v}) )$, e é definido como:


$\mathbf{u} \cdot \mathbf{v} = \|\mathbf{u}\| \times \|\mathbf{v}\| \times \cos(\theta)$


onde $( \theta )$ é o ângulo entre $( \mathbf{u} ) e ( \mathbf{v}).$

Outra maneira de calcular o produto escalar é:



$\mathbf{u} \cdot \mathbf{v} = \sum_{i} u_i \times v_]$

**Em Python**

O produto escalar é bastante simples de implementar:

```python
def dot_product(v1, v2):
    return sum(v1_i * v2_i for v1_i, v2_i in zip(v1, v2))

dot_product(u, v)
```

Mas uma implementação muito mais eficiente é fornecida pelo NumPy com a função `np.dot()`:

```python
np.dot(u, v)
```

Equivalentemente, você pode usar o método `dot` dos `ndarray`s:

```python
u.dot(v)
```

**Cuidado:** o operador `*` realizará uma multiplicação elemento por elemento, **NÃO** um produto escalar:

```python
print(" ", u)
print("* ", v, "(NÃO é um produto escalar)")
print("-"*10)
print(u * v)
```

**Propriedades principais**

- O produto escalar é **comutativo**: $( \mathbf{u} \cdot \mathbf{v} = \mathbf{v} \cdot \mathbf{u} )$.
- O produto escalar é definido apenas entre dois vetores, não entre um escalar e um vetor. Isso significa que não podemos encadear produtos escalares: por exemplo, a expressão $( \mathbf{u} \cdot \mathbf{v} \cdot \mathbf{w} )$ não é definida, pois $( \mathbf{u} \cdot \mathbf{v} )$ é um escalar e $( \mathbf{w} )$ é um vetor.
- Isso também significa que o produto escalar **NÃO** é associativo: $( (\mathbf{u} \cdot \mathbf{v}) \cdot \mathbf{w} \neq \mathbf{u} \cdot (\mathbf{v} \cdot \mathbf{w}) )$, já que nenhum dos dois é definido.
- No entanto, o produto escalar é **associativo em relação à multiplicação por escalar**: $( \lambda \times (\mathbf{u} \cdot \mathbf{v}) = (\lambda \times \mathbf{u}) \cdot \mathbf{v} = \mathbf{u} \cdot (\lambda \times \mathbf{v}) )$.
- Finalmente, o produto escalar é **distributivo** sobre a adição de vetores: $( \mathbf{u} \cdot (\mathbf{v} + \mathbf{w}) = \mathbf{u} \cdot \mathbf{v} + \mathbf{u} \cdot \mathbf{w} )$.

---

### **Calculando o ângulo entre vetores**

Um dos muitos usos do produto escalar é calcular o ângulo entre dois vetores não nulos. Olhando para a definição do produto escalar, podemos deduzir a seguinte fórmula:


$\theta = \arccos \left( \frac{\mathbf{u} \cdot \mathbf{v}}{\|\mathbf{u}\| \times \|\mathbf{v}\|} \right)$


Observe que se $( \mathbf{u} \cdot \mathbf{v} = 0 ),$ segue que ( $\theta = \frac{\pi}{2} )$. Em outras palavras, se o produto escalar de dois vetores não nulos for zero, isso significa que eles são ortogonais.

Vamos usar essa fórmula para calcular o ângulo entre $( \mathbf{u} )$ e $( \mathbf{v} )$ (em radianos):

```python
def vector_angle(u, v):
    cos_theta = u.dot(v) / LA.norm(u) / LA.norm(v)
    return np.arccos(cos_theta.clip(-1, 1))

theta = vector_angle(u, v)
print("Ângulo =", theta, "radianos")
print("    =", theta * 180 / np.pi, "graus")
```

**Nota:** devido a pequenos erros de ponto flutuante, `cos_theta` pode estar ligeiramente fora do intervalo \([-1, 1]\), o que faria o `arccos` falhar. É por isso que cortamos o valor dentro do intervalo usando a função `clip` do NumPy.

---

### **Projetando um ponto em um eixo**

O produto escalar também é muito útil para projetar pontos em um eixo. A projeção do vetor $( \mathbf{v} )$ no eixo de $( \mathbf{u} )$ é dada por esta fórmula:


$\text{proj}_{\mathbf{u}} \mathbf{v} = \frac{\mathbf{u} \cdot \mathbf{v}}{\|\mathbf{u}\|^2} \times \mathbf{u}$


O que é equivalente a:


$\text{proj}_{\mathbf{u}} \mathbf{v} = (\mathbf{v} \cdot \hat{\mathbf{u}}) \times \hat{\mathbf{u}}$


```python
u_normalized = u / LA.norm(u)
proj = v.dot(u_normalized) * u_normalized

plot_vector2d(u, color="r")
plot_vector2d(v, color="b")

plot_vector2d(proj, color="k", linestyle=":")
plt.plot(proj[0], proj[1], "ko")

plt.plot([proj[0], v[0]], [proj[1], v[1]], "b:")

plt.text(1, 2, "$proj_u v$", color="k", fontsize=18)
plt.text(1.8, 0.2, "$v$", color="b", fontsize=18)
plt.text(0.8, 3, "$u$", color="r", fontsize=18)

plt.axis([0, 8, 0, 5.5])
plt.gca().set_aspect("equal")
plt.grid()
#plt.show()
plt.savefig('/home/paulo/Obsidian Vault/Obsidian Vault/plot/plot3d.png')
```

---

### **Matrizes**

Uma matriz é um array retangular de escalares (ou seja, qualquer número: inteiro, real ou complexo) organizados em linhas e colunas, por exemplo:

$$

\begin{bmatrix}
10 & 20 & 30 \\
40 & 50 & 60
\end{bmatrix}


$$
Você também pode pensar em uma matriz como uma lista de vetores: a matriz anterior contém 2 vetores horizontais 3D ou 3 vetores verticais 2D.

As matrizes são convenientes e muito eficientes para realizar operações em muitos vetores de uma só vez. Também veremos que elas são ótimas para representar e realizar transformações lineares, como rotações, translações e escalonamentos.

---

### **Matrizes em Python**

Em Python, uma matriz pode ser representada de várias maneiras. A mais simples é apenas uma lista de listas do Python:

```python
[
    [10, 20, 30],
    [40, 50, 60]
]
```

Uma maneira muito mais eficiente é usar a biblioteca NumPy, que fornece implementações otimizadas de muitas operações matriciais:

```python
A = np.array([
    [10, 20, 30],
    [40, 50, 60]
])
print(A)
```

Por convenção, as matrizes geralmente têm nomes em maiúsculas, como \( A \).

No restante deste tutorial, assumiremos que estamos usando arrays NumPy (tipo `ndarray`) para representar matrizes.

---

### **Tamanho**

O tamanho de uma matriz é definido pelo número de linhas e colunas. Ele é denotado por  
linhas × colunas. Por exemplo, a matriz ( A ) acima é um exemplo de uma matriz ( 2 $\times$ 3 ): 2 linhas, 3 colunas. **Cuidado:** uma matriz ( 3 $\times$ 2 ) teria 3 linhas e 2 colunas.

Para obter o tamanho de uma matriz no NumPy:

```python
A.shape
```

**Cuidado:** o atributo `size` representa o número de elementos no `ndarray`, não o tamanho da matriz:

```python
A.size
```

---

### **Indexação de elementos**

O número localizado na $( i^{ésima} )$ linha e $( j^{ésima})$ coluna de uma matriz ( X\) é às vezes denotado por ( X_{i,j}\) ou ( X_{ij} ), mas não há uma notação padrão, então as pessoas geralmente preferem nomear explicitamente os elementos, assim: "seja $( X = (x_{i,j})_{1 \leq i \leq m, 1 \leq j \leq n} )$." Isso significa que ( X ) é igual a:

$$

X = 
\begin{bmatrix}
x_{1,1} & x_{1,2} & x_{1,3} & \cdots & x_{1,n} \\
x_{2,1} & x_{2,2} & x_{2,3} & \cdots & x_{2,n} \\
x_{3,1} & x_{3,2} & x_{3,3} & \cdots & x_{3,n} \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
x_{m,1} & x_{m,2} & x_{m,3} & \cdots & x_{m,n}
\end{bmatrix}


$$
No entanto, neste notebook, usaremos a notação $( X_{i,j} )$, pois ela corresponde bem à notação do NumPy. Observe que, em matemática, os índices geralmente começam em 1, mas na programação eles geralmente começam em 0. Portanto, para acessar $( A_{2,3} )$ programaticamente, precisamos escrever:

```python
A[1, 2]  # 2ª linha, 3ª coluna
```

O vetor da $( i^{ésima} )$ linha é às vezes denotado por $( M_i)$ ou $( M_{i,*} )$, mas novamente não há uma notação padrão, então as pessoas geralmente preferem definir seus próprios nomes, por exemplo: "seja ( x_i) o vetor da ( $i^{ésima}$ ) linha da matriz ( X )." Usaremos ( M_{i,*} ) pelo mesmo motivo mencionado acima. Por exemplo, para acessar ( A_{2,*} ) (ou seja, o vetor da 2ª linha de ( A ):

```python
A[1, :]  # vetor da 2ª linha (como um array 1D)
```

Da mesma forma, o vetor da ( $j^{ésima}$ ) coluna é às vezes denotado por ( M^j ) ou \( M_{*,j} ), mas não há uma notação padrão. Usaremos ( M_{*,j} ). Por exemplo, para acessar ( A_{*,3} ) (ou seja, o vetor da 3ª coluna de ( A )):

```python
A[:, 2]  # vetor da 3ª coluna (como um array 1D)
```

Observe que o resultado é na verdade um array NumPy unidimensional: não existe algo como um array unidimensional vertical ou horizontal. Se você precisar realmente representar um vetor linha como uma matriz de uma linha (ou seja, um array 2D do NumPy) ou um vetor coluna como uma matriz de uma coluna, então você precisa usar um slice em vez de um inteiro ao acessar a linha ou coluna, por exemplo:

```python
print(A[1:2, :])  # linhas 2 a 3 (excluído): isso retorna a linha 2 como uma matriz de uma linha
print(A[:, 2:3])  # colunas 3 a 4 (excluído): isso retorna a coluna 3 como uma matriz de uma coluna
```

---

### **Matrizes quadradas, triangulares, diagonais e identidade**

Uma **matriz quadrada** é uma matriz que tem o mesmo número de linhas e colunas, por exemplo, uma matriz \( 3 \times 3 \):

$$

\begin{bmatrix}
4 & 9 & 2 \\
3 & 5 & 7 \\
8 & 1 & 6
\end{bmatrix}
$$


Uma **matriz triangular superior** é um tipo especial de matriz quadrada onde todos os elementos abaixo da diagonal principal (do canto superior esquerdo para o canto inferior direito) são zero, por exemplo:

$$

\begin{bmatrix}
4 & 9 & 2 \\
0 & 5 & 7 \\
0 & 0 & 6
\end{bmatrix}

$$

Da mesma forma, uma **matriz triangular inferior** é uma matriz quadrada onde todos os elementos acima da diagonal principal são zero, por exemplo:

$$

\begin{bmatrix}
4 & 0 & 0 \\
3 & 5 & 0 \\
8 & 1 & 6
\end{bmatrix}
$$


Uma **matriz triangular** é aquela que é triangular superior ou inferior.

Uma matriz que é tanto triangular superior quanto inferior é chamada de **matriz diagonal**, por exemplo:

$$

\begin{bmatrix}
4 & 0 & 0 \\
0 & 5 & 0 \\
0 & 0 & 6
\end{bmatrix}
$$


Você pode construir uma matriz diagonal usando a função `diag` do NumPy:

```python
np.diag([4, 5, 6])
```

Se você passar uma matriz para a função `diag`, ela extrairá os valores da diagonal:

```python
D = np.array([
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
])
print(np.diag(D))
```

Finalmente, a **matriz identidade** de tamanho ( n ), denotada por $( I_n )$, é uma matriz diagonal de tamanho $( n \times n )$ com 1s na diagonal principal, por exemplo \( I_3 \):

$$

\begin{bmatrix}
1 & 0 & 0 \\
0 & 1 & 0 \\
0 & 0 & 1
\end{bmatrix}

$$

A função `eye` do NumPy retorna a matriz identidade do tamanho desejado:

```python
np.eye(3)
```

A matriz identidade é frequentemente denotada simplesmente por \( I \) (em vez de \( I_n \)) quando seu tamanho é claro dado o contexto. Ela é chamada de matriz identidade porque multiplicar uma matriz por ela deixa a matriz inalterada, como veremos abaixo.

---

### **Adição de matrizes**

Se duas matrizes \( Q \) e \( R \) tiverem o mesmo tamanho $( m \times n )$, elas podem ser somadas. A adição é realizada elemento por elemento: o resultado também é uma matriz $( m \times n ) ( S )$ onde cada elemento é a soma dos elementos na posição correspondente:

$$

S_{i,j} = Q_{i,j} + R_{i,j}
$$$$


S = 
\begin{bmatrix}
Q_{11} + R_{11} & Q_{12} + R_{12} & Q_{13} + R_{13} & \cdots & Q_{1n} + R_{1n} \\
Q_{21} + R_{21} & Q_{22} + R_{22} & Q_{23} + R_{23} & \cdots & Q_{2n} + R_{2n} \\
Q_{31} + R_{31} & Q_{32} + R_{32} & Q_{33} + R_{33} & \cdots & Q_{3n} + R_{3n} \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
Q_{m1} + R_{m1} & Q_{m2} + R_{m2} & Q_{m3} + R_{m3} & \cdots & Q_{mn} + R_{mn}
\end{bmatrix}

$$

Por exemplo, vamos criar uma matriz $( 2 \times 3 )$ ( B ) e calcular ( A + B ):

```python
import numpy as np
B = np.array([
    [1, 2, 3],
    [4, 5, 6]
])
A = np.array([
       [10, 20, 30],
       [40, 50, 60]
])

print(A + B)
```




A adição é comutativa, o que significa que \( A + B = B + A \):

```python
print(B + A)
```

Também é associativa, o que significa que \( A + (B + C) = (A + B) + C \):

```python
C = np.array([
    [100, 200, 300],
    [400, 500, 600]
])
print(A + (B + C))

print((A + B) + C)
```

---

### **Multiplicação por escalar**

Uma matriz ( M ) pode ser multiplicada por um escalar $( \lambda)$. O resultado é denotado por $( \lambda M )$, e é uma matriz do mesmo tamanho que \( M \) com todos os elementos multiplicados por $( \lambda ):$


$$
\lambda M = 
\begin{bmatrix}
\lambda \times M_{11} & \lambda \times M_{12} & \lambda \times M_{13} & \cdots & \lambda \times M_{1n} \\
\lambda \times M_{21} & \lambda \times M_{22} & \lambda \times M_{23} & \cdots & \lambda \times M_{2n} \\
\lambda \times M_{31} & \lambda \times M_{32} & \lambda \times M_{33} & \cdots & \lambda \times M_{3n} \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
\lambda \times M_{m1} & \lambda \times M_{m2} & \lambda \times M_{m3} & \cdots & \lambda \times M_{mn}
\end{bmatrix}
$$


Uma maneira mais concisa de escrever isso é:

$$

(\lambda M)_{i,j} = \lambda(M)_{i,j}

$$

No NumPy, basta usar o operador `*` para multiplicar uma matriz por um escalar. Por exemplo:

```python
2 * A
```

A multiplicação por escalar também é definida no lado direito e dá o mesmo resultado: $( M\lambda = \lambda M )$. Por exemplo:

```python
A * 2
```

Isso torna a multiplicação por escalar **comutativa**.

Também é **associativa**, o que significa que $( \alpha (\beta M)$ = $(\alpha \times \beta)M )$, onde ( $\alpha )$ e ( $\beta$ ) são escalares. Por exemplo:

```python
print(2 * (3 * A))

print((2 * 3) * A)
```

Finalmente, é **distributiva** sobre a adição de matrizes, o que significa que $( \lambda(Q + R) = \lambda Q + \lambda R )$:

```python
print(2 * (A + B))

print( 2 * A + 2 * B)
```





---

### **Multiplicação de matrizes**

Até agora, as operações matriciais foram bastante intuitivas. Mas multiplicar matrizes é um pouco mais complexo.

Uma matriz ( Q ) de tamanho $( m \times n )$ pode ser multiplicada por uma matriz ( R ) de tamanho $( n \times q )$. Ela é denotada simplesmente por ( QR ) sem sinal de multiplicação ou ponto. O resultado ( P ) é uma matriz $( m \times q )$ onde cada elemento é calculado como uma soma de produtos:

$P_{i,j} = \sum_{k=1}^n{Q_{i,k} \times R_{k,j}}$

O elemento na posição \( i, j \) na matriz resultante é a soma dos produtos dos elementos na linha \( i \) da matriz \( Q \) pelos elementos na coluna \( j \) da matriz \( R \).


$$
 P =
\begin{bmatrix}
Q_{11} R_{11} + Q_{12} R_{21} + \cdots + Q_{1n} R_{n1} &
  Q_{11} R_{12} + Q_{12} R_{22} + \cdots + Q_{1n} R_{n2} &
    \cdots &
      Q_{11} R_{1q} + Q_{12} R_{2q} + \cdots + Q_{1n} R_{nq} \\
Q_{21} R_{11} + Q_{22} R_{21} + \cdots + Q_{2n} R_{n1} &
  Q_{21} R_{12} + Q_{22} R_{22} + \cdots + Q_{2n} R_{n2} &
    \cdots &
      Q_{21} R_{1q} + Q_{22} R_{2q} + \cdots + Q_{2n} R_{nq} \\
  \vdots & \vdots & \ddots & \vdots \\
Q_{m1} R_{11} + Q_{m2} R_{21} + \cdots + Q_{mn} R_{n1} &
  Q_{m1} R_{12} + Q_{m2} R_{22} + \cdots + Q_{mn} R_{n2} &
    \cdots &
      Q_{m1} R_{1q} + Q_{m2} R_{2q} + \cdots + Q_{mn} R_{nq}
\end{bmatrix} 
$$

Você pode notar que cada elemento $( P_{i,j} )$ é o produto escalar do vetor linha ( $Q_{i,*} )$ e do vetor coluna $( R_{*,j}):$

$P_{i,j} = Q_{i,*} \cdot R_{*,j}$

Portanto, podemos reescrever \( P \) de forma mais concisa como:

$$P =
\begin{bmatrix}
Q_{1,*} \cdot R_{*,1} & Q_{1,*} \cdot R_{*,2} & \cdots & Q_{1,*} \cdot R_{*,q} \\
Q_{2,*} \cdot R_{*,1} & Q_{2,*} \cdot R_{*,2} & \cdots & Q_{2,*} \cdot R_{*,q} \\
\vdots & \vdots & \ddots & \vdots \\
Q_{m,*} \cdot R_{*,1} & Q_{m,*} \cdot R_{*,2} & \cdots & Q_{m,*} \cdot R_{*,q}
\end{bmatrix}$$


Vamos multiplicar duas matrizes no NumPy, usando a função `np.matmul()`:

```python
D = np.array([
        [ 2,  3,  5,  7],
        [11, 13, 17, 19],
        [23, 29, 31, 37]
    ])
E = np.matmul(A, D)
print(E) 
```

O Python 3.5 introduziu o operador infixo `@` para multiplicação de matrizes, e o NumPy 1.10 adicionou suporte para ele. `A @ D` é equivalente a `np.matmul(A, D)`:

```python
print(A @ D )
```

O operador `@` também funciona para vetores. `u @ v` calcula o produto escalar de `u` e `v`:

```python
u @ v
```

Vamos verificar esse resultado olhando para um elemento, só para ter certeza. Para calcular $( E_{2,3} )$, por exemplo, precisamos multiplicar os elementos na 2ª linha de ( A) pelos elementos na 3ª coluna de ( D) e somar esses produtos:

```python
40*5 + 50*17 + 60*31

print(E[1, 2])  # linha 2, coluna 3
```


Parece bom! Você pode verificar os outros elementos até se acostumar com o algoritmo.

Multiplicamos uma matriz $( 2 \times 3 )$ por uma matriz $( 3 \times 4 )$, então o resultado é uma matriz $( 2 \times 4 )$. O número de colunas da primeira matriz deve ser igual ao número de linhas da segunda matriz. Se tentarmos multiplicar ( D ) por ( A ), obteremos um erro porque ( D ) tem 4 colunas enquanto ( A ) tem 2 linhas:

```python
try:
    D @ A
except ValueError as e:
    print("ValueError:", e)
```

Isso ilustra o fato de que a multiplicação de matrizes **NÃO** é comutativa: em geral, $( QR \neq RQ )$.

Na verdade, ( QR ) e ( RQ ) são ambos definidos apenas se ( Q) tiver tamanho $( m \times n )$ e ( R ) tiver tamanho $( n \times m )$. Vamos ver um exemplo onde ambos são definidos e mostrar que eles (em geral) **NÃO** são iguais:

```python
F = np.array([
        [5,2],
        [4,1],
        [9,3]
    ])
print(A @ F)
```


```python
print(F @ A )
```
Por outro lado, a **multiplicação de matrizes é associativa**, o que significa que \( Q(RS) = (QR)S \). Vamos criar uma matriz $( 4 \times 5 )$ ( G ) para ilustrar isso:

```python
G = np.array([
    [8, 7, 4, 2, 5],
    [2, 5, 1, 0, 5],
    [9, 11, 17, 21, 0],
    [0, 1, 0, 1, 2]
])

print((A @ D) @ G)  # (AD)G

print(A @ (D @ G))  # A(DG)
```

Também é **distributiva** sobre a adição de matrizes, o que significa que \( (Q + R)S = QS + RS \). Por exemplo:

```python
print((A + B) @ D)

print(A @ D + B @ D)
```

O produto de uma matriz \( M \) pela matriz identidade (de tamanho correspondente) resulta na mesma matriz \( M \). Mais formalmente, se \( M \) é uma matriz $( m \times n )$, então:


$MI_n = I_mM = M$


Isso geralmente é escrito de forma mais concisa (já que o tamanho das matrizes identidade é inequívoco dado o contexto):


$MI = IM = M$


Por exemplo:

```python
print(A @ np.eye(3))

print(np.eye(2) @ A)
```


**Cuidado:** O operador `*` do NumPy realiza multiplicação elemento por elemento, **NÃO** uma multiplicação de matrizes:

```python
A * B  # NÃO é uma multiplicação de matrizes
```

---

### **Transposta de uma matriz**

A transposta de uma matriz \( M \) é uma matriz denotada por $( M^T )$ tal que a $( i^{ésima} )$ linha em $( M^T )$ é igual à $( i^{ésima} )$ coluna em $( M )$:

$$
A^T = 
\begin{bmatrix}
10 & 20 & 30 \\
40 & 50 & 60
\end{bmatrix}^T =
\begin{bmatrix}
10 & 40 \\
20 & 50 \\
30 & 60
\end{bmatrix}
$$

Em outras palavras, $( (A^T)_{i,j} = A_{j,i})$.

Obviamente, se $( M )$ é uma matriz $( m \times n )$, então $( M^T )$ é uma matriz $( n \times m )$.

**Nota:** existem algumas outras notações, como $( M^t ), ( M' )$ ou $( tM )$.

No NumPy, a transposta de uma matriz pode ser obtida simplesmente usando o atributo `T`:

```python
A

print(A.T)
```

Como você pode esperar, transpor uma matriz duas vezes retorna a matriz original:

```python
print(A.T.T)
```

A transposição é distributiva sobre a adição de matrizes, o que significa que $( (Q + R)^T = Q^T + R^T )$. Por exemplo:

```python
print((A + B).T)

print( A.T + B.T)
```

Além disso, $( (Q \cdot R)^T = R^T \cdot Q^T )$. Observe que a ordem é invertida. Por exemplo:

```python
print((A @ D).T)

print( D.T @ A.T)
```

---

### **Matriz simétrica**

Uma **matriz simétrica** é definida como uma matriz que é igual à sua transposta: \( M = M^T \). Essa definição implica que ela deve ser uma matriz quadrada cujos elementos são simétricos em relação à diagonal principal, por exemplo:


$$
\begin{bmatrix}
17 & 22 & 27 & 49 \\
22 & 29 & 36 & 0 \\
27 & 36 & 45 & 2 \\
49 & 0 & 2 & 99
\end{bmatrix}

$$

O produto de uma matriz por sua transposta é sempre uma matriz simétrica, por exemplo:

```python
print(D @ D.T)
```

Como mencionamos anteriormente, no NumPy (ao contrário do Matlab, por exemplo), 1D realmente significa 1D: não existe algo como um array 1D vertical ou horizontal. Portanto, você não deve se surpreender ao ver que transpor um array 1D não faz nada:

```python
print(u)

print(u.T)
```

Queremos converter `u` em um vetor linha antes de transpor. Existem algumas maneiras de fazer isso:

```python
u_row = np.array([u])
print(u_row)
```

Observe os colchetes extras: isso é um array 2D com apenas uma linha (ou seja, uma matriz $( 1 \times 2 )$. Em outras palavras, é realmente um vetor linha.

```python
u[np.newaxis, :]
```

Isso é bastante explícito: estamos pedindo um novo eixo vertical, mantendo os dados existentes como o eixo horizontal.

```python
u[np.newaxis]
```

Isso é equivalente, mas um pouco menos explícito.

```python
u[None]
```

Essa é a versão mais curta, mas você provavelmente deve evitá-la porque não é clara. A razão pela qual funciona é que `np.newaxis` é na verdade igual a `None`, então isso é equivalente à versão anterior.

Agora vamos transpor nosso vetor linha:

```python
u_row.T
```

Ótimo! Agora temos um vetor coluna.

Em vez de criar um vetor linha e depois transpô-lo, também é possível converter um array 1D diretamente em um vetor coluna:

```python
u[:, np.newaxis]
```

---

### **Plotando uma matriz**

Já vimos que vetores podem ser representados como pontos ou setas em um espaço N-dimensional. Existe uma boa representação gráfica de matrizes? Bem, você pode simplesmente ver uma matriz como uma lista de vetores, então plotar uma matriz resulta em muitos pontos ou setas. Por exemplo, vamos criar uma matriz $( 2 \times 4 )$ ( P) e plotá-la como pontos:

```python
import matplotlib.pyplot as plt
P = np.array([
    [3.0, 4.0, 1.0, 4.6],
    [0.2, 3.5, 2.0, 0.5]
])

x_coords_P, y_coords_P = P
plt.scatter(x_coords_P, y_coords_P)
plt.axis([0, 5, 0, 4])
plt.gca().set_aspect("equal")
plt.grid()
plt.show()
```

Claro, também poderíamos ter armazenado os mesmos 4 vetores como vetores linha em vez de vetores coluna, resultando em uma matriz $( 4 \times 2 )$ (a transposta de( P ), na verdade). É realmente uma escolha arbitrária.

Como os vetores são ordenados, você pode ver a matriz como um caminho e representá-la com pontos conectados:

```python
plt.plot(x_coords_P, y_coords_P, "bo")
plt.plot(x_coords_P, y_coords_P, "b--")
plt.axis([0, 5, 0, 4])
plt.gca().set_aspect("equal")
plt.grid()
plt.show()
```

Ou você pode representá-la como um polígono: a classe `Polygon` do `matplotlib` espera um array NumPy $( n \times 2 )$, não um array $( 2 \times n )$, então precisamos apenas passar $( P^T )$:

```python
from matplotlib.patches import Polygon
plt.gca().add_artist(Polygon(P.T))
plt.axis([0, 5, 0, 4])
plt.gca().set_aspect("equal")
plt.grid()
plt.show()
```

---

### **Aplicações geométricas de operações matriciais**

Vimos anteriormente que a adição de vetores resulta em uma translação geométrica, a multiplicação de vetores por um escalar resulta em redimensionamento (zoom in ou out, centrado na origem), e o produto escalar de vetores resulta na projeção de um vetor em outro vetor, redimensionando e medindo a coordenada resultante.

Da mesma forma, as operações matriciais têm aplicações geométricas muito úteis.

---

### **Adição = múltiplas translações geométricas**

Primeiro, adicionar duas matrizes é equivalente a adicionar todos os seus vetores. Por exemplo, vamos criar uma matriz $( 2 \times 4 )$ ( H) e adicioná-la a ( P ), e observar o resultado:

```python
H = np.array([
    [0.5, -0.2, 0.2, -0.1],
    [0.4, 0.4, 1.5, 0.6]
])

P_moved = P + H

plt.gca().add_artist(Polygon(P.T, alpha=0.2))
plt.gca().add_artist(Polygon(P_moved.T, alpha=0.2))
for vector, origin in zip(H.T, P.T):
    plot_vector2d(vector, origin=origin)

plt.text(2.2, 1.8, "$P$", color="b", fontsize=18)
plt.text(2.0, 3.2, "$P+H$", color="r", fontsize=18)
plt.text(2.5, 0.5, "$H_{*,1}$", color="k", fontsize=18)
plt.text(4.1, 3.5, "$H_{*,2}$", color="k", fontsize=18)
plt.text(0.4, 2.6, "$H_{*,3}$", color="k", fontsize=18)
plt.text(4.3, 0.2, "$H_{*,4}$", color="k", fontsize=18)

plt.axis([0, 5, 0, 4])
plt.gca().set_aspect("equal")
plt.grid()
plt.show()
```

Se adicionarmos uma matriz cheia de vetores idênticos, obteremos uma simples translação geométrica:

```python
H2 = np.array([
    [-0.5, -0.5, -0.5, -0.5],
    [0.4, 0.4, 0.4, 0.4]
])

P_translated = P + H2

plt.gca().add_artist(Polygon(P.T, alpha=0.2))
plt.gca().add_artist(Polygon(P_translated.T, alpha=0.3, color="r"))
for vector, origin in zip(H2.T, P.T):
    plot_vector2d(vector, origin=origin)

plt.axis([0, 5, 0, 4])
plt.gca().set_aspect("equal")
plt.grid()
plt.show()
```

Embora as matrizes só possam ser somadas se tiverem o mesmo tamanho, o NumPy permite adicionar um vetor linha ou coluna a uma matriz: isso é chamado de **broadcasting** e é explicado em mais detalhes no tutorial do NumPy. Poderíamos ter obtido o mesmo resultado acima com:

```python
P + [[-0.5], [0.4]]  # mesmo que P + H2, graças ao broadcasting do NumPy
```

---

### **Multiplicação por escalar**

Multiplicar uma matriz por um escalar resulta em todos os seus vetores sendo multiplicados por esse escalar, então, sem surpresa, o resultado geométrico é um redimensionamento de toda a figura. Por exemplo, vamos redimensionar nosso polígono por um fator de 60% (zoom out, centrado na origem):

```python
def plot_transformation(P_before, P_after, text_before, text_after, axis=[0, 5, 0, 4], arrows=True):
    if arrows:
        for vector_before, vector_after in zip(P_before.T, P_after.T):
            plot_vector2d(vector_before, color="blue", linestyle="--")
            plot_vector2d(vector_after, color="red", linestyle="-")
    plt.gca().add_artist(Polygon(P_before.T, alpha=0.2))
    plt.gca().add_artist(Polygon(P_after.T, alpha=0.3, color="r"))
    plt.plot(P_before[0], P_before[1], "b--", alpha=0.5)
    plt.plot(P_after[0], P_after[1], "r--", alpha=0.5)
    plt.text(P_before[0].mean(), P_before[1].mean(), text_before, fontsize=18, color="blue")
    plt.text(P_after[0].mean(), P_after[1].mean(), text_after, fontsize=18, color="red")
    plt.axis(axis)
    plt.gca().set_aspect("equal")
    plt.grid()

P_rescaled = 0.60 * P
plot_transformation(P, P_rescaled, "$P$", "$0.6 P$", arrows=True)
plt.show()
```

---

### **Multiplicação de matrizes – Projeção em um eixo**

A multiplicação de matrizes é mais complexa de visualizar, mas também é a ferramenta mais poderosa disponível.

Vamos começar de forma simples, definindo uma matriz $( 1 \times 2 ) ( U = [1 \quad 0] )$. Esse vetor linha é apenas o vetor unitário horizontal.

```python
U = np.array([[1, 0]])
```

Agora vamos olhar para o produto escalar $( U \cdot P )$:

```python
U @ P
```

Essas são as coordenadas horizontais dos vetores em \( P \). Em outras palavras, acabamos de projetar \( P \) no eixo horizontal:

```python
def plot_projection(U, P):
    U_P = U @ P
    
    axis_end = 100 * U
    plot_vector2d(axis_end[0], color="black")

    plt.gca().add_artist(Polygon(P.T, alpha=0.2))
    for vector, proj_coordinate in zip(P.T, U_P.T):
        proj_point = proj_coordinate * U
        plt.plot(proj_point[0][0], proj_point[0][1], "ro", zorder=10)
        plt.plot([vector[0], proj_point[0][0]], [vector[1], proj_point[0][1]],
                 "r--", zorder=10)

    plt.axis([0, 5, 0, 4])
    plt.gca().set_aspect("equal")
    plt.grid()
    plt.show()

plot_projection(U, P)
```

Podemos realmente projetar em qualquer outro eixo, basta substituir \( U \) por qualquer outro vetor unitário. Por exemplo, vamos projetar no eixo que está a um ângulo de $( 30^\circ )$ acima do eixo horizontal:

```python
angle30 = 30 * np.pi / 180  # ângulo em radianos
U_30 = np.array([[np.cos(angle30), np.sin(angle30)]])

plot_projection(U_30, P)
```

Bom! Lembre-se de que o produto escalar de um vetor unitário e uma matriz basicamente realiza uma projeção em um eixo e nos dá as coordenadas dos pontos resultantes nesse eixo.

---

### **Multiplicação de matrizes – Rotação**

Agora vamos criar uma matriz $( 2 \times 2 ) ( V )$ contendo dois vetores unitários que fazem ângulos de $( 30^\circ ) e ( 120^\circ )$ com o eixo horizontal:

$$

V = 
\begin{bmatrix}
\cos(30^\circ) & \sin(30^\circ) \\
\cos(120^\circ) & \sin(120^\circ)
\end{bmatrix}

$$

```python
import numpy as np
angle120 = 120 * np.pi / 180
V = np.array([
    [np.cos(angle30), np.sin(angle30)],
    [np.cos(angle120), np.sin(angle120)]
])

print(V)
```

Vamos olhar para o produto \( VP \):

```python
print(V @ P)
```

A primeira linha é igual a $( V_{1,*}P)$, que são as coordenadas da projeção de ( P ) no eixo de $( 30^\circ )$, como vimos acima. A segunda linha é $( V_{2,*}P )$, que são as coordenadas da projeção de $( P )$ no eixo de $( 120^\circ )$. Então, basicamente, obtivemos as coordenadas de $( P )$ após girar os eixos horizontal e vertical em $( 30^\circ )$ (ou equivalentemente, após girar o polígono em $(-30^\circ)$ em torno da origem)! Vamos plotar $( VP )$ para ver isso:

```python
P_rotated = V @ P
plot_transformation(P, P_rotated, "$P$", "$VP$", [-2, 6, -2, 4], arrows=True)
plt.show()
```

A matriz \( V \) é chamada de **matriz de rotação**.

---

### **Multiplicação de matrizes – Outras transformações lineares**

De forma mais geral, qualquer transformação linear \( f \) que mapeie vetores n-dimensionais para vetores m-dimensionais pode ser representada como uma matriz $( m \times n )$. Por exemplo, digamos que $( \mathbf{u} )$ é um vetor tridimensional:

$$

\mathbf{u} = 
\begin{pmatrix}
x \\
y \\
z
\end{pmatrix}

$$

e \( f \) é definida como:


$$
f(\mathbf{u}) =
\begin{pmatrix}
a x + b y + c z \\
d x + e y + f z
\end{pmatrix}

$$
Essa transformação \( f \) mapeia vetores tridimensionais para vetores bidimensionais de forma linear (ou seja, as coordenadas resultantes envolvem apenas somas de múltiplos das coordenadas originais). Podemos representar essa transformação como a matriz \( F \):
$$


F = 
\begin{bmatrix}
a & b & c \\
d & e & f
\end{bmatrix}
$$


Agora, para calcular $( f(\mathbf{u}) )$, podemos simplesmente fazer uma multiplicação de matrizes:


$f(\mathbf{u}) = F \mathbf{u}$


Se tivermos uma matriz $( G = [\mathbf{u}_1 \quad \mathbf{u}_2 \quad \cdots \quad \mathbf{u}_q] )$, onde cada $( \mathbf{u}_i )$ é um vetor coluna tridimensional, então $( FG )$ resulta na transformação linear de todos os vetores $( \mathbf{u}_i )$ conforme definido pela matriz ( F ):


$FG = [f(\mathbf{u}_1) \quad f(\mathbf{u}_2) \quad \cdots \quad f(\mathbf{u}_q)]$


Para resumir, a matriz no lado esquerdo de um produto escalar especifica qual transformação linear aplicar aos vetores do lado direito. Já mostramos que isso pode ser usado para realizar projeções e rotações, mas qualquer outra transformação linear é possível. Por exemplo, aqui está uma transformação conhecida como **mapeamento de cisalhamento**:

```python
F_shear = np.array([
    [1, 1.5],
    [0, 1]
])

plot_transformation(P, F_shear @ P, "$P$", "$F_{\text{shear}} P$", axis=[0, 10, 0, 7])
plt.show()
```

Vamos ver como essa transformação afeta o quadrado unitário:

```python
Square = np.array([
    [0, 0, 1, 1],
    [0, 1, 1, 0]
])

plot_transformation(Square, F_shear @ Square, "$Square$", "$F_{\text{shear}} Square$", axis=[0, 2.6, 0, 1.8])
plt.show()
```

Agora vamos ver um **mapeamento de compressão**:

```python
F_squeeze = np.array([
    [1.4, 0],
    [0, 1/1.4]
])

plot_transformation(P, F_squeeze @ P, "$P$", "$F_{\text{squeeze}} P$", axis=[0, 7, 0, 5])
plt.show()
```

O efeito no quadrado unitário é:

```python
plot_transformation(Square, F_squeeze @ Square, "$Square$", "$F_{\text{squeeze}} Square$", axis=[0, 1.8, 0, 1.2])
plt.show()
```

Vamos mostrar mais uma – reflexão através do eixo horizontal:

```python
F_reflect = np.array([
    [1, 0],
    [0, -1]
])

plot_transformation(P, F_reflect @ P, "$P$", "$F_{\text{reflect}} P$", axis=[-2, 9, -4.5, 4.5])
plt.show()
```

---

### **Inversa de uma matriz**

Agora que entendemos que uma matriz pode representar qualquer transformação linear, uma pergunta natural é: podemos encontrar uma matriz de transformação que reverta o efeito de uma determinada matriz de transformação \( F \)? A resposta é sim... às vezes! Quando existe, essa matriz é chamada de **inversa** de \( F \), e é denotada por $( F^{-1} )$.

Por exemplo, a rotação, o mapeamento de cisalhamento e o mapeamento de compressão acima têm transformações inversas. Vamos demonstrar isso no mapeamento de cisalhamento:

```python
F_inv_shear = np.array([
    [1, -1.5],
    [0, 1]
])

P_sheared = F_shear @ P
P_unsheared = F_inv_shear @ P_sheared
plot_transformation(P_sheared, P_unsheared, "$P_{\text{sheared}}$", "$P_{\text{unsheared}}$", axis=[0, 10, 0, 7])
plt.plot(P[0], P[1], "b--")
plt.show()
```

Aplicamos um mapeamento de cisalhamento em \( P \), como fizemos antes, mas então aplicamos uma segunda transformação ao resultado, e _voilà_, isso teve o efeito de voltar ao \( P \) original (plotei o contorno do \( P \) original para verificar). A segunda transformação é a inversa da primeira.

Definimos a matriz inversa $( F^{-1}_{\text{shear}} )$ manualmente desta vez, mas o NumPy fornece uma função `inv` para calcular a inversa de uma matriz, então poderíamos ter escrito:

```python
F_inv_shear = LA.inv(F_shear)
F_inv_shear
```

Apenas matrizes quadradas podem ser invertidas. Isso faz sentido quando você pensa sobre isso: se você tiver uma transformação que reduz o número de dimensões, então algumas informações são perdidas e não há como recuperá-las. Por exemplo, digamos que você use uma matriz $( 2 \times 3 )$ para projetar um objeto 3D em um plano. O resultado pode parecer com isso:

```python
plt.plot([0, 0, 1, 1, 0, 0.1, 0.1, 0, 0.1, 1.1, 1.0, 1.1, 1.1, 1.0, 1.1, 0.1],
         [0, 1, 1, 0, 0, 0.1, 1.1, 1.0, 1.1, 1.1, 1.0, 1.1, 0.1, 0, 0.1, 0.1],
         "r-")
plt.axis([-0.5, 2.1, -0.5, 1.5])
plt.gca().set_aspect("equal")
plt.grid()
plt.show()
```

Olhando para essa imagem, é impossível dizer se isso é a projeção de um cubo ou a projeção de um objeto retangular estreito. Algumas informações foram perdidas na projeção.

Mesmo matrizes de transformação quadradas podem perder informações. Por exemplo, considere esta matriz de transformação:

```python
F_project = np.array([
    [1, 0],
    [0, 0]
])

plot_transformation(P, F_project @ P, "$P$", r"$F_{\text{project}} \cdot P$", axis=[0, 6, -1, 4])
plt.show()
```

Essa matriz de transformação realiza uma projeção no eixo horizontal. Nosso polígono é completamente achatado, então algumas informações são totalmente perdidas, e é impossível voltar ao polígono original usando uma transformação linear. Em outras palavras, $( F_{\text{project}})$ não tem inversa. Tal matriz quadrada que não pode ser invertida é chamada de **matriz singular** (também conhecida como matriz degenerada). Se pedirmos ao NumPy para calcular sua inversa, ele levantará uma exceção:

```python
try:
    LA.inv(F_project)
except LA.LinAlgError as e:
    print("LinAlgError:", e)
```

Aqui está outro exemplo de uma matriz singular. Esta realiza uma projeção no eixo a um ângulo de $( 30^\circ )$ acima do eixo horizontal:

```python
angle30 = 30 * np.pi / 180
F_project_30 = np.array([
    [np.cos(angle30)**2, np.sin(2*angle30)/2],
    [np.sin(2*angle30)/2, np.sin(angle30)**2]
])

plot_transformation(P, F_project_30 @ P, "$P$", r"$F_{\text{project\_30}} \cdot P$", axis=[0, 6, -1, 4])
plt.show()
```

Mas desta vez, devido a erros de arredondamento de ponto flutuante, o NumPy consegue calcular uma inversa (observe como os elementos são grandes, no entanto):

```python
LA.inv(F_project_30)
```

Como você pode esperar, o produto escalar de uma matriz por sua inversa resulta na matriz identidade:


$M \cdot M^{-1} = M^{-1} \cdot M = I$


Isso faz sentido, já que fazer uma transformação linear seguida pela transformação inversa resulta em nenhuma mudança.

```python
F_shear @ LA.inv(F_shear)
```

Outra maneira de expressar isso é que a inversa da inversa de uma matriz \( M \) é \( M \) mesma:


$((M)^{-1})^{-1} = M$


```python
LA.inv(LA.inv(F_shear))
```

Além disso, a inversa de escalonar por um fator de $( \lambda )$ é, é claro, escalonar por um fator de $( \frac{1}{\lambda} )$:


$(\lambda \times M)^{-1} = \frac{1}{\lambda} \times M^{-1}$


Assim que você entende a interpretação geométrica das matrizes como transformações lineares, a maioria dessas propriedades parece bastante intuitiva.

Uma matriz que é sua própria inversa é chamada de **involução**. Os exemplos mais simples são matrizes de reflexão, ou uma rotação de $( 180^\circ )$, mas também existem involuções mais complexas, por exemplo, imagine uma transformação que comprime horizontalmente, depois reflete sobre o eixo vertical e finalmente gira $( 90^\circ )$ no sentido horário. Pegue um guardanapo e tente fazer isso duas vezes: você acabará na posição original. Aqui está a matriz involutória correspondente:

```python
F_involution = np.array([
    [0, -2],
    [-1/2, 0]
])

plot_transformation(P, F_involution @ P, "$P$", r"$F_{\text{involution}} \cdot P$", axis=[-8, 5, -4, 4])
plt.show()
```

Finalmente, uma matriz quadrada \( H \) cuja inversa é sua própria transposta é uma **matriz ortogonal**:


$H^{-1} = H^T$


Portanto:


$H \cdot H^T = H^T \cdot H = I$


Ela corresponde a uma transformação que preserva distâncias, como rotações e reflexões, e combinações dessas, mas não redimensionamento, cisalhamento ou compressão. Vamos verificar que $( F_{\text{reflect}} )$ é de fato ortogonal:

```python
F_reflect @ F_reflect.T
```

---

### **Determinante**

O determinante de uma matriz quadrada \( M \), denotado por $( \det(M) )$ ou $( \det M )$ ou $( |M| )$, é um valor que pode ser calculado a partir de seus elementos $( (M_{i,j}) )$ usando vários métodos equivalentes. Um dos métodos mais simples é esta abordagem recursiva:

$$

|M| = M_{1,1} \times |M^{(1,1)}| - M_{1,2} \times |M^{(1,2)}| + M_{1,3} \times |M^{(1,3)}| - M_{1,4} \times |M^{(1,4)}| + \cdots \pm M_{1,n} \times |M^{(1,n)}|

$$

- Onde $( M^{(i,j)} )$ é a matriz ( M ) sem a linha ( i ) e a coluna ( j ).

Por exemplo, vamos calcular o determinante da seguinte matriz $( 3 \times 3 )$:

$$

M = 
\begin{bmatrix}
1 & 2 & 3 \\
4 & 5 & 6 \\
7 & 8 & 0
\end{bmatrix}
$$

Usando o método acima, obtemos:

$$

|M| = 1 \times |[5 \quad 6 \quad 8 \quad 0]| - 2 \times |[4 \quad 6 \quad 7 \quad 0]| + 3 \times |[4 \quad 5 \quad 7 \quad 8]|

$$

Agora precisamos calcular o determinante de cada uma dessas matrizes $( 2 \times 2 )$ (esses determinantes são chamados de menores):

$$

\begin{vmatrix}
5 & 6 \\
8 & 0
\end{vmatrix} = 5 \times 0 - 6 \times 8 = -48

$$


$$
\begin{vmatrix}
4 & 6 \\
7 & 0
\end{vmatrix} = 4 \times 0 - 6 \times 7 = -42
$$


$$

\begin{vmatrix}
4 & 5 \\
7 & 8
\end{vmatrix} = 4 \times 8 - 5 \times 7 = -3
$$


Agora podemos calcular o resultado final:


$|M| = 1 \times (-48) - 2 \times (-42) + 3 \times (-3) = 27$


Para obter o determinante de uma matriz, você pode chamar a função `det` do NumPy no módulo `numpy.linalg`:

```python
M = np.array([
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 0]
])

LA.det(M)
```

Um dos principais usos do determinante é determinar se uma matriz quadrada pode ser invertida ou não: se o determinante for igual a 0, então a matriz não pode ser invertida (é uma matriz singular), e se o determinante não for 0, então ela pode ser invertida.

Por exemplo, vamos calcular o determinante para as matrizes $( F_{\text{project}} ), ( F_{\text{project\_30}} ) e ( F_{\text{shear}} )$ que definimos anteriormente:

```python
LA.det(F_project)
```

Isso mesmo, $( F_{\text{project}} )$ é singular, como vimos anteriormente.

```python
LA.det(F_project_30)
```

Esse determinante está suspeitamente próximo de 0: ele realmente deveria ser 0, mas não é devido a pequenos erros de ponto flutuante. A matriz é na verdade singular.

```python
LA.det(F_shear)
```

Perfeito! Essa matriz pode ser invertida, como vimos anteriormente. Uau, a matemática realmente funciona!

O determinante também pode ser usado para medir o quanto uma transformação linear afeta as áreas de superfície: por exemplo, as matrizes de projeção $( F_{\text{project}} ) e ( F_{\text{project\_30}} )$ achatam completamente o polígono \( P \), até que sua área seja zero. É por isso que o determinante dessas matrizes é 0. O mapeamento de cisalhamento modificou a forma do polígono, mas não afetou sua área de superfície, e é por isso que o determinante é 1. Você pode tentar calcular o determinante de uma matriz de rotação e também deve encontrar 1. E quanto a uma matriz de redimensionamento? Vamos ver:

```python
F_scale = np.array([
    [0.5, 0],
    [0, 0.5]
])

plot_transformation(P, F_scale @ P, "$P$", r"$F_{\text{scale}} \cdot P$", axis=[0, 6, -1, 4])
plt.show()
```

Redimensionamos o polígono por um fator de 1/2 em ambos os eixos vertical e horizontal, então a área de superfície do polígono resultante é \( 1/4 \) da área do polígono original. Vamos calcular o determinante e verificar isso:

```python
LA.det(F_scale)
```

Correto!

O determinante pode realmente ser negativo, quando a transformação resulta em uma versão "invertida" do polígono original (por exemplo, uma luva para a mão esquerda se torna uma luva para a mão direita). Por exemplo, o determinante da matriz $( F_{\text{reflect}})$ é -1 porque a área de superfície é preservada, mas o polígono é invertido:

```python
LA.det(F_reflect)
```

---

### **Compondo transformações lineares**

Várias transformações lineares podem ser encadeadas simplesmente realizando múltiplos produtos escalares em sequência. Por exemplo, para realizar um mapeamento de compressão seguido por um mapeamento de cisalhamento, basta escrever:

```python
P_squeezed_then_sheared = F_shear @ (F_squeeze @ P)
```

Como o produto escalar é associativo, o seguinte código é equivalente:

```python
P_squeezed_then_sheared = F_shear @ F_squeeze @ P
```

Observe que a ordem das transformações é o inverso da ordem do produto escalar.

Se formos realizar essa composição de transformações lineares mais de uma vez, podemos salvar a matriz de composição assim:

```python
F_squeeze_then_shear = F_shear @ F_squeeze
P_squeezed_then_sheared = F_squeeze_then_shear @ P
```

A partir de agora, podemos realizar ambas as transformações em apenas um produto escalar, o que pode levar a um aumento significativo no desempenho.

E se você quiser realizar o inverso dessa dupla transformação? Bem, se você comprimiu e depois cisalhou, e quer desfazer o que fez, deve ser óbvio que você deve primeiro "descisalhar" e depois "descomprimir". Em termos mais matemáticos, dadas duas matrizes invertíveis (também conhecidas como não singulares) \( Q \) e \( R \):

$$
[
(Q \cdot R)^{-1} = R^{-1} \cdot Q^{-1}
]

$$
E no NumPy:

```python
LA.inv(F_shear @ F_squeeze) == LA.inv(F_squeeze) @ LA.inv(F_shear)
```

---

### **Decomposição em Valores Singulares (SVD)**

Acontece que qualquer matriz $( m \times n ) ( M )$ pode ser decomposta no produto escalar de três matrizes simples:

- Uma matriz de rotação \( U \) (uma matriz ortogonal $( m \times m ))$
- Uma matriz de escalonamento e projeção $( \Sigma )$ (uma matriz diagonal $( m \times n ))$
- E outra matriz de rotação \( V^T \) (uma matriz ortogonal $( n \times n ))$

$$
[
M = U \cdot \Sigma \cdot V^T
]
$$

Por exemplo, vamos decompor a transformação de cisalhamento:

```python
U, S_diag, V_T = LA.svd(F_shear)  # nota: no Python 3, você pode renomear S_diag para Σ_diag
S_diag
```

Observe que isso é apenas um array 1D contendo os valores diagonais de $( \Sigma )$. Para obter a matriz $( \Sigma )$ real, podemos usar a função `diag` do NumPy:

```python
S = np.diag(S_diag)
S
```

Agora vamos verificar que $( U \cdot \Sigma \cdot V^T )$ é de fato igual a $( F_{\text{shear}} ):$

```python
U @ np.diag(S_diag) @ V_T

F_shear
```

Funcionou perfeitamente. Vamos aplicar essas transformações uma por uma (em ordem inversa) no quadrado unitário para entender o que está acontecendo. Primeiro, vamos aplicar a primeira rotação $( V^T )$:

```python
plot_transformation(Square, V_T @ Square, "$Square$", r"$V^T \cdot Square$", axis=[-0.5, 3.5, -1.5, 1.5])
plt.show()
```

Agora vamos redimensionar ao longo dos eixos vertical e horizontal usando $( \Sigma ):$

```python
plot_transformation(V_T @ Square, S @ V_T @ Square, r"$V^T \cdot Square$", r"$\Sigma \cdot V^T \cdot Square$", axis=[-0.5, 3.5, -1.5, 1.5])
plt.show()
```

Finalmente, aplicamos a segunda rotação \( U \):

```python
plot_transformation(S @ V_T @ Square, U @ S @ V_T @ Square, r"$\Sigma \cdot V^T \cdot Square$", r"$U \cdot \Sigma \cdot V^T \cdot Square$", axis=[-0.5, 3.5, -1.5, 1.5])
plt.show()
```

E podemos ver que o resultado é de fato um mapeamento de cisalhamento do quadrado unitário original.

---

### **Autovetores e autovalores**

Um **autovetor** de uma matriz quadrada \( M \) (também chamado de **vetor característico**) é um vetor não nulo que permanece na mesma linha após a transformação pela transformação linear associada a \( M \). Uma definição mais formal é qualquer vetor $( \mathbf{v} )$ tal que:

$$
[
M \cdot \mathbf{v} = \lambda \times \mathbf{v}
]
$$

Onde $( \lambda )$ é um valor escalar chamado de **autovalor** associado ao vetor $( \mathbf{v} )$.

Por exemplo, qualquer vetor horizontal permanece horizontal após aplicar o mapeamento de cisalhamento (como você pode ver na imagem acima), então ele é um autovetor de $( M ).$ Um vetor vertical acaba inclinado para a direita, então vetores verticais **NÃO** são autovetores de $( M).$

Se olharmos para o mapeamento de compressão, descobrimos que qualquer vetor horizontal ou vertical mantém sua direção (embora seu comprimento mude), então todos os vetores horizontais e verticais são autovetores de ( $F_{\text{squeeze}} )$.

No entanto, matrizes de rotação não têm autovetores (exceto se o ângulo de rotação for ( $0^\circ) ou ( 180^\circ )$, caso em que todos os vetores não nulos são autovetores).

A função `eig` do NumPy retorna a lista de autovetores unitários e seus autovalores correspondentes para qualquer matriz quadrada. Vamos ver os autovetores e autovalores da matriz de compressão $( F_{\text{squeeze}} )$:

```python
eigenvalues, eigenvectors = LA.eig(F_squeeze)
eigenvalues  # [λ0, λ1, …]

eigenvectors  # [v0, v1, …]
```

De fato, os vetores horizontais são esticados por um fator de 1,4, e os vetores verticais são comprimidos por um fator de 1/1,4=0,714..., então tudo bem. Vamos ver a matriz de cisalhamento \( F_{\text{shear}} \):

```python
eigenvalues2, eigenvectors2 = LA.eig(F_shear)
eigenvalues2  # [λ0, λ1, …]

eigenvectors2  # [v0, v1, …]
```

Espere, o quê!? Esperávamos apenas um autovetor unitário, não dois. O segundo vetor é quase igual a \( \begin{pmatrix} -1 \\ 0 \end{pmatrix} \), que está na mesma linha que o primeiro vetor \( \begin{pmatrix} 1 \\ 0 \end{pmatrix} \). Isso se deve a erros de ponto flutuante. Podemos ignorar com segurança vetores que são (quase) colineares (ou seja, na mesma linha).

---

### **Traço**

O traço de uma matriz quadrada \( M \), denotado por $( \text{tr}(M) )$, é a soma dos valores em sua diagonal principal. Por exemplo:

```python
D = np.array([
    [100, 200, 300],
    [10, 20, 30],
    [1, 2, 3]
])

D.trace()
```

O traço não tem uma interpretação geométrica simples (em geral), mas tem várias propriedades que o tornam útil em muitas áreas:
$$

- ( \text{tr}(A + B) = \text{tr}(A) + \text{tr}(B) )
- ( \text{tr}(A \cdot B) = \text{tr}(B \cdot A) )
- ( \text{tr}(A \cdot B \cdot \cdots Y \cdot Z) = \text{tr}(Z \cdot A \cdot B \cdot \cdots Y) )
- ( \text{tr}(A^T \cdot B) = \text{tr}(A \cdot B^T) = \text{tr}(B^T \cdot A) = text{tr}(B \cdot A^T) = \sum_{i,j} X_{i,j} \times Y_{i,j})
$$

No entanto, ele tem uma interpretação geométrica útil no caso de matrizes de projeção (como ( $F_{\text{project}} )$ que discutimos anteriormente): ele corresponde ao número de dimensões após a projeção. Por exemplo:

```python
F_project.trace()
```

---





