---
Atualizado: 2025-03-26  15.36
Criado: 2025-03-26  15.36
---
**Apêndice D – Autodiferenciação**  

_Este notebook contém implementações simples de várias técnicas de autodiferenciação para explicar como elas funcionam._  

<table align="left">  
  <td>  
    <a href="https://colab.research.google.com/github/ageron/handson-ml3/blob/main/extra_autodiff.ipynb" target="_parent"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Abrir no Colab"/></a>  
  </td>  
  <td>  
    <a target="_blank" href="https://kaggle.com/kernels/welcome?src=https://github.com/ageron/handson-ml3/blob/main/extra_autodiff.ipynb"><img src="https://kaggle.com/static/images/open-in-kaggle.svg" /></a>  
  </td>  
</table>  

# Configuração  

# Introdução  

Suponha que queremos calcular os gradientes da função \( f(x,y) = x^2 y + y + 2 \) em relação aos parâmetros \( x \) e \( y \):  

```python  
def f(x, y):  
    return x * x * y + y + 2  
```  

Uma abordagem é resolver isso analiticamente:  

\[  
\frac{\partial f}{\partial x} = 2xy  
\]  

\[  
\frac{\partial f}{\partial y} = x^2 + 1  
\]  

```python  
def df(x, y):  
    return 2 * x * y, x * x + 1  
```  

Assim, por exemplo, \( \frac{\partial f}{\partial x}(3,4) = 24 \) e \( \frac{\partial f}{\partial y}(3,4) = 10 \).  

```python  
df(3, 4)  
```  

**Saída:**  
```  
(24, 10)  
```  

Perfeito! Também podemos encontrar as equações para as derivadas de segunda ordem (também chamadas de Hessianas):  

\[  
\frac{\partial^2 f}{\partial x^2} = \frac{\partial (2xy)}{\partial x} = 2y  
\]  

\[  
\frac{\partial^2 f}{\partial x \partial y} = \frac{\partial (2xy)}{\partial y} = 2x  
\]  

\[  
\frac{\partial^2 f}{\partial y \partial x} = \frac{\partial (x^2 + 1)}{\partial x} = 2x  
\]  

\[  
\frac{\partial^2 f}{\partial y^2} = \frac{\partial (x^2 + 1)}{\partial y} = 0  
\]  

Para \( x = 3 \) e \( y = 4 \), essas Hessianas são, respectivamente, 8, 6, 6, 0. Vamos usar as equações acima para calculá-las:  

```python  
def d2f(x, y):  
    return [2 * y, 2 * x], [2 * x, 0]  
```  

```python  
d2f(3, 4)  
```  

**Saída:**  
```  
([8, 6], [6, 0])  
```  

Perfeito, mas isso exige algum trabalho matemático. Não é muito difícil neste caso, mas para uma rede neural profunda, é praticamente impossível calcular as derivadas dessa maneira. Então, vamos explorar várias formas de automatizar esse processo!  

# Diferenciação Numérica  

Aqui, calculamos uma aproximação dos gradientes usando a equação:  

\[  
\frac{\partial f}{\partial x} = \lim_{\epsilon \to 0} \frac{f(x + \epsilon, y) - f(x, y)}{\epsilon}  
\]  

(e há uma definição similar para \( \frac{\partial f}{\partial y} \)).  

```python  
def gradients(func, vars_list, eps=0.0001):  
    partial_derivatives = []  
    base_func_eval = func(*vars_list)  
    for idx in range(len(vars_list)):  
        tweaked_vars = vars_list[:]  
        tweaked_vars[idx] += eps  
        tweaked_func_eval = func(*tweaked_vars)  
        derivative = (tweaked_func_eval - base_func_eval) / eps  
        partial_derivatives.append(derivative)  
    return partial_derivatives  
```  

```python  
def df(x, y):  
    return gradients(f, [x, y])  
```  

```python  
df(3, 4)  
```  

**Saída:**  
```  
[24.000400000048216, 10.000000000047748]  
```  

Funciona bem!  

A boa notícia é que é bastante fácil calcular as Hessianas. Primeiro, vamos criar funções que calculam as derivadas parciais de primeira ordem (também chamadas de Jacobianas):  

```python  
def dfdx(x, y):  
    return gradients(f, [x, y])[0]  

def dfdy(x, y):  
    return gradients(f, [x, y])[1]  

dfdx(3.0, 4.0), dfdy(3.0, 4.0)  
```  

**Saída:**  
```  
(24.000400000048216, 10.000000000047748)  
```  

Agora podemos simplesmente aplicar a função `gradients()` a essas funções:  

```python  
def d2f(x, y):  
    return [gradients(dfdx, [x, y]), gradients(dfdy, [x, y])]  
```  

```python  
d2f(3, 4)  
```  

**Saída:**  
```  
[[7.999999951380232, 6.000099261882497],  
 [6.000099261882497, -1.4210854715202004e-06]]  
```  

Tudo funciona bem, mas o resultado é aproximado, e calcular os gradientes de uma função em relação a \( n \) variáveis requer chamar essa função \( n \) vezes. Em redes neurais profundas, muitas vezes há milhares de parâmetros para ajustar usando gradiente descendente (o que requer calcular os gradientes da função de perda em relação a cada um desses parâmetros), então essa abordagem seria muito lenta.  

## Implementando um Grafo de Computação Simples  

Em vez dessa abordagem numérica, vamos implementar algumas técnicas de autodiferenciação simbólica. Para isso, precisaremos definir classes para representar constantes, variáveis e operações.  

```python  
class Const(object):  
    def __init__(self, value):  
        self.value = value  
    def evaluate(self):  
        return self.value  
    def __str__(self):  
        return str(self.value)  

class Var(object):  
    def __init__(self, name, init_value=0):  
        self.value = init_value  
        self.name = name  
    def evaluate(self):  
        return self.value  
    def __str__(self):  
        return self.name  

class BinaryOperator(object):  
    def __init__(self, a, b):  
        self.a = a  
        self.b = b  

class Add(BinaryOperator):  
    def evaluate(self):  
        return self.a.evaluate() + self.b.evaluate()  
    def __str__(self):  
        return f"{self.a} + {self.b}"  

class Mul(BinaryOperator):  
    def evaluate(self):  
        return self.a.evaluate() * self.b.evaluate()  
    def __str__(self):  
        return f"({self.a}) * ({self.b})"  
```  

Ótimo, agora podemos construir um grafo de computação para representar a função \( f \):  

```python  
x = Var("x")  
y = Var("y")  
f = Add(Mul(Mul(x, x), y), Add(y, Const(2)))  # f(x,y) = x²y + y + 2  
```  

E podemos executar esse grafo para calcular \( f \) em qualquer ponto, por exemplo, \( f(3, 4) \).  

```python  
x.value = 3  
y.value = 4  
f.evaluate()  
```  

**Saída:**  
```  
42  
```  

Perfeito, encontramos a resposta definitiva.  

## Calculando Gradientes  

Os métodos de autodiferenciação que apresentaremos abaixo são baseados na *regra da cadeia*.  

Suponha que temos duas funções \( u \) e \( v \), e as aplicamos sequencialmente a alguma entrada \( x \), obtendo o resultado \( z \). Então, temos \( z = v(u(x)) \), que podemos reescrever como \( z = v(s) \) e \( s = u(x) \). Agora, podemos aplicar a regra da cadeia para obter a derivada parcial da saída \( z \) em relação à entrada \( x \):  

\[  
\frac{\partial z}{\partial x} = \frac{\partial s}{\partial x} \cdot \frac{\partial z}{\partial s}  
\]  

Se \( z \) for a saída de uma sequência de funções com saídas intermediárias \( s_1, s_2, \dots, s_n \), a regra da cadeia ainda se aplica:  

\[  
\frac{\partial z}{\partial x} = \frac{\partial s_1}{\partial x} \cdot \frac{\partial s_2}{\partial s_1} \cdot \frac{\partial s_3}{\partial s_2} \cdot \dots \cdot \frac{\partial s_n}{\partial s_{n-1}} \cdot \frac{\partial z}{\partial s_n}  
\]  

Na autodiferenciação em modo direto, o algoritmo calcula esses termos "para frente" (ou seja, na mesma ordem que os cálculos necessários para computar a saída \( z \)), da esquerda para a direita: primeiro \( \frac{\partial s_1}{\partial x} \), depois \( \frac{\partial s_2}{\partial s_1} \), e assim por diante. Na autodiferenciação em modo reverso, o algoritmo calcula esses termos "para trás", da direita para a esquerda: primeiro \( \frac{\partial z}{\partial s_n} \), depois \( \frac{\partial s_n}{\partial s_{n-1}} \), e assim por diante.  

Por exemplo, suponha que você queira calcular a derivada da função \( z(x) = \sin(x^2) \) em \( x = 3 \), usando autodiferenciação em modo direto. O algoritmo primeiro calcularia a derivada parcial \( \frac{\partial s_1}{\partial x} = \frac{\partial x^2}{\partial x} = 2x = 6 \). Em seguida, calcularia \( \frac{\partial z}{\partial x} = \frac{\partial s_1}{\partial x} \cdot \frac{\partial z}{\partial s_1} = 6 \cdot \frac{\partial \sin(s_1)}{\partial s_1} = 6 \cdot \cos(s_1) = 6 \cdot \cos(3^2) \approx -5.46 \).  

Vamos verificar esse resultado usando a função `gradients()` definida anteriormente:  

```python  
from math import sin  

def z(x):  
    return sin(x**2)  

gradients(z, [3])  
```  

**Saída:**  
```  
[-5.46761419430053]  
```  

Parece bom. Agora vamos fazer o mesmo usando autodiferenciação em modo reverso. Desta vez, o algoritmo começaria pelo lado direito, calculando \( \frac{\partial z}{\partial s_1} = \frac{\partial \sin(s_1)}{\partial s_1} = \cos(s_1) = \cos(3^2) \approx -0.91 \). Em seguida, calcularia \( \frac{\partial z}{\partial x} = \frac{\partial s_1}{\partial x} \cdot \frac{\partial z}{\partial s_1} \approx \frac{\partial s_1}{\partial x} \cdot -0.91 = \frac{\partial x^2}{\partial x} \cdot -0.91 = 2x \cdot -0.91 = 6 \cdot -0.91 = -5.46 \).  

Claro, ambas as abordagens fornecem o mesmo resultado (exceto por erros de arredondamento), e com uma única entrada e saída, elas envolvem o mesmo número de cálculos. Mas quando há várias entradas ou saídas, elas podem ter desempenhos muito diferentes. De fato, se houver muitas entradas, os termos mais à direita serão necessários para calcular as derivadas parciais em relação a cada entrada, então é uma boa ideia calcular esses termos primeiro. Isso significa usar autodiferenciação em modo reverso. Dessa forma, os termos mais à direita podem ser calculados apenas uma vez e usados para calcular todas as derivadas parciais. Por outro lado, se houver muitas saídas, o modo direto geralmente é preferível, pois os termos mais à esquerda podem ser calculados apenas uma vez para calcular as derivadas parciais das diferentes saídas. No Deep Learning, geralmente há milhares de parâmetros do modelo, ou seja, muitas entradas, mas poucas saídas. Na verdade, geralmente há apenas uma saída durante o treinamento: a perda. É por isso que o modo reverso é usado no TensorFlow e em todas as principais bibliotecas de Deep Learning.  

Há uma complexidade adicional no modo reverso: o valor de \( s_i \) geralmente é necessário ao calcular \( \frac{\partial s_{i+1}}{\partial s_i} \), e calcular \( s_i \) requer primeiro calcular \( s_{i-1} \), que requer calcular \( s_{i-2} \), e assim por diante. Basicamente, é necessário um primeiro passe para frente pela rede para calcular \( s_1, s_2, \dots, s_n \), e então o algoritmo pode calcular as derivadas parciais da direita para a esquerda. Armazenar todos os valores intermediários \( s_i \) na RAM às vezes é um problema, especialmente ao lidar com imagens e ao usar GPUs, que geralmente têm RAM limitada. Para limitar esse problema, pode-se reduzir o número de camadas na rede neural ou configurar o TensorFlow para trocar esses valores da RAM da GPU para a RAM da CPU. Outra abordagem é armazenar apenas alguns valores intermediários, como \( s_1, s_3, s_5, \dots, s_{n-4}, s_{n-2}, s_n \). Isso significa que, quando o algoritmo calcular as derivadas parciais, se um valor intermediário \( s_i \) estiver faltando, ele precisará ser recalculado com base no valor intermediário anterior \( s_{i-1} \). Isso troca CPU por RAM (se você estiver interessado, confira [este artigo](https://pdfs.semanticscholar.org/f61e/9fd5a4878e1493f7a6b03774a61c17b7e9a4.pdf)).  

### Autodiferenciação em modo direto  

```python  
Const.gradient = lambda self, var: Const(0)  
Var.gradient = lambda self, var: Const(1) if self is var else Const(0)  
Add.gradient = lambda self, var: Add(self.a.gradient(var), self.b.gradient(var))  
Mul.gradient = lambda self, var: Add(Mul(self.a, self.b.gradient(var)), Mul(self.a.gradient(var), self.b))  

x = Var(name="x", init_value=3.0)  
y = Var(name="y", init_value=4.0)  
f = Add(Mul(Mul(x, x), y), Add(y, Const(2)))  # f(x,y) = x²y + y + 2  

dfdx = f.gradient(x)  # 2xy  
dfdy = f.gradient(y)  # x² + 1  
```  

```python  
dfdx.evaluate(), dfdy.evaluate()  
```  

**Saída:**  
```  
(24.0, 10.0)  
```  

Como a saída do método `gradient()` é totalmente simbólica, não estamos limitados a derivadas de primeira ordem; podemos também calcular derivadas de segunda ordem e assim por diante:  

```python  
d2fdxdx = dfdx.gradient(x)  # 2y  
d2fdxdy = dfdx.gradient(y)  # 2x  
d2fdydx = dfdy.gradient(x)  # 2x  
d2fdydy = dfdy.gradient(y)  # 0  
```  

```python  
[[d2fdxdx.evaluate(), d2fdxdy.evaluate()],  
 [d2fdydx.evaluate(), d2fdydy.evaluate()]]  
```  

**Saída:**  
```  
[[8.0, 6.0], [6.0, 0.0]]  
```  

Observe que o resultado agora é exato, não uma aproximação (dentro dos limites da precisão de ponto flutuante da máquina, é claro).  

### Autodiferenciação em modo direto usando números duais  

Uma maneira elegante de aplicar autodiferenciação em modo direto é usar [números duais](https://en.wikipedia.org/wiki/Dual_number). Resumidamente, um número dual \( z \) tem a forma \( z = a + b\epsilon \), onde \( a \) e \( b \) são números reais, e \( \epsilon \) é um número infinitesimal, positivo, mas menor que todos os números reais, e tal que \( \epsilon^2 = 0 \). Pode-se mostrar que \( f(x + \epsilon) = f(x) + \frac{\partial f}{\partial x}\epsilon \), então simplesmente calculando \( f(x + \epsilon) \), obtemos tanto o valor de \( f(x) \) quanto a derivada parcial de \( f \) em relação a \( x \).  

Os números duais têm suas próprias regras aritméticas, que geralmente são bastante naturais. Por exemplo:  

**Adição**  
\[  
(a_1 + b_1\epsilon) + (a_2 + b_2\epsilon) = (a_1 + a_2) + (b_1 + b_2)\epsilon  
\]  

**Subtração**  
\[  
(a_1 + b_1\epsilon) - (a_2 + b_2\epsilon) = (a_1 - a_2) + (b_1 - b_2)\epsilon  
\]  

**Multiplicação**  
\[  
(a_1 + b_1\epsilon) \times (a_2 + b_2\epsilon) = (a_1 a_2) + (a_1 b_2 + a_2 b_1)\epsilon  
\]  

**Divisão**  
\[  
\frac{a_1 + b_1\epsilon}{a_2 + b_2\epsilon} = \frac{a_1}{a_2} + \frac{a_1 b_2 - b_1 a_2}{a_2^2}\epsilon  
\]  

**Potência**  
\[  
(a + b\epsilon)^n = a^n + (n a^{n-1}b)\epsilon  
\]  

Vamos criar uma classe para representar números duais e implementar algumas operações (adição e multiplicação). Você pode tentar adicionar mais se quiser.  

```python  
class DualNumber(object):  
    def __init__(self, value=0.0, eps=0.0):  
        self.value = value  
        self.eps = eps  
    def __add__(self, b):  
        return DualNumber(self.value + self.to_dual(b).value,  
                         self.eps + self.to_dual(b).eps)  
    def __radd__(self, a):  
        return self.to_dual(a).__add__(self)  
    def __mul__(self, b):  
        return DualNumber(self.value * self.to_dual(b).value,  
                         self.eps * self.to_dual(b).value + self.value * self.to_dual(b).eps)  
    def __rmul__(self, a):  
        return self.to_dual(a).__mul__(self)  
    def __str__(self):  
        if self.eps:  
            return f"{self.value:.1f} + {self.eps:.1f}ε"  
        else:  
            return f"{self.value:.1f}"  
    def __repr__(self):  
        return str(self)  
    @classmethod  
    def to_dual(cls, n):  
        if hasattr(n, "value"):  
            return n  
        else:  
            return cls(n)  
```  

\( 3 + (3 + 4\epsilon) = 6 + 4\epsilon \)  

```python  
3 + DualNumber(3, 4)  
```  

**Saída:**  
```  
6.0 + 4.0ε  
```  

\( (3 + 4\epsilon) \times (5 + 7\epsilon) = 15 + 41\epsilon \)  

```python  
DualNumber(3, 4) * DualNumber(5, 7)  
```  

**Saída:**  
```  
15.0 + 41.0ε  
```  

Agora vamos ver se os números duais funcionam com nosso framework de computação simples:  

```python  
x.value = DualNumber(3.0)  
y.value = DualNumber(4.0)  

f.evaluate()  
```  

**Saída:**  
```  
42.0  
```  

Sim, funciona! Agora vamos usá-lo para calcular as derivadas parciais de \( f \) em relação a \( x \) e \( y \) em \( x = 3 \) e \( y = 4 \):  

```python  
x.value = DualNumber(3.0, 1.0)  # 3 + ε  
y.value = DualNumber(4.0)        # 4  

dfdx = f.evaluate().eps  

x.value = DualNumber(3.0)        # 3  
y.value = DualNumber(4.0, 1.0)   # 4 + ε  

dfdy = f.evaluate().eps  
```  

```python  
dfdx  
```  

**Saída:**  
```  
24.0  
```  

```python  
dfdy  
```  

**Saída:**  
```  
10.0  
```  

Ótimo! No entanto, nesta implementação, estamos limitados a derivadas de primeira ordem. Agora vamos ver o modo reverso.  

### Autodiferenciação em modo reverso  

Vamos reescrever nosso framework simples para adicionar autodiferenciação em modo reverso:  

```python  
class Const(object):  
    def __init__(self, value):  
        self.value = value  
    def evaluate(self):  
        return self.value  
    def backpropagate(self, gradient):  
        pass  
    def __str__(self):  
        return str(self.value)  

class Var(object):  
    def __init__(self, name, init_value=0):  
        self.value = init_value  
        self.name = name  
        self.gradient = 0  
    def evaluate(self):  
        return self.value  
    def backpropagate(self, gradient):  
        self.gradient += gradient  
    def __str__(self):  
        return self.name  

class BinaryOperator(object):  
    def __init__(self, a, b):  
        self.a = a  
        self.b = b  

class Add(BinaryOperator):  
    def evaluate(self):  
        self.value = self.a.evaluate() + self.b.evaluate()  
        return self.value  
    def backpropagate(self, gradient):  
        self.a.backpropagate(gradient)  
        self.b.backpropagate(gradient)  
    def __str__(self):  
        return f"{self.a} + {self.b}"  

class Mul(BinaryOperator):  
    def evaluate(self):  
        self.value = self.a.evaluate() * self.b.evaluate()  
        return self.value  
    def backpropagate(self, gradient):  
        self.a.backpropagate(gradient * self.b.value)  
        self.b.backpropagate(gradient * self.a.value)  
    def __str__(self):  
        return f"({self.a}) * ({self.b})"  
```  

```python  
x = Var("x", init_value=3)  
y = Var("y", init_value=4)  
f = Add(Mul(Mul(x, x), y), Add(y, Const(2)))  # f(x,y) = x²y + y + 2  

result = f.evaluate()  
f.backpropagate(1.0)  
```  

```python  
print(f)  
```  

**Saída:**  
```  
((x) * (x)) * (y) + y + 2  
```  

```python  
result  
```  

**Saída:**  
```  
42  
```  

```python  
x.gradient  
```  

**Saída:**  
```  
24.0  
```  

```python  
y.gradient  
```  

**Saída:**  
```  
10.0  
```  

Novamente, nesta implementação, as saídas são apenas números, não expressões simbólicas, então estamos limitados a derivadas de primeira ordem. No entanto, poderíamos ter feito os métodos `backpropagate()` retornarem expressões simbólicas em vez de valores (por exemplo, retornar `Add(2, 3)` em vez de 5). Isso tornaria possível calcular gradientes de segunda ordem (e além). É isso que o TensorFlow faz, assim como todas as principais bibliotecas que implementam autodiferenciação.  

### Autodiferenciação em modo reverso usando TensorFlow  

```python  
import tensorflow as tf  
```  

```python  
x = tf.Variable(3.0)  
y = tf.Variable(4.0)  

with tf.GradientTape() as tape:  
    f = x * x * y + y + 2  

jacobians = tape.gradient(f, [x, y])  
jacobians  
```  

**Saída:**  
```  
[<tf.Tensor: shape=(), dtype=float32, numpy=24.0>,  
 <tf.Tensor: shape=(), dtype=float32, numpy=10.0>]  
```  

Como tudo é simbólico, podemos calcular derivadas de segunda ordem e além:  

```python  
x = tf.Variable(3.0)  
y = tf.Variable(4.0)  

with tf.GradientTape(persistent=True) as tape:  
    f = x * x * y + y + 2  
    df_dx, df_dy = tape.gradient(f, [x, y])  

d2f_dx2, d2f_dydx = tape.gradient(df_dx, [x, y])  
d2f_dxdy, d2f_dy2 = tape.gradient(df_dy, [x, y])  
del tape  

hessians = [[d2f_dx2, d2f_dydx], [d2f_dxdy, d2f_dy2]]  
hessians  
```  

**Aviso:**  
```  
WARNING:tensorflow: Chamar GradientTape.gradient em uma fita persistente dentro de seu contexto é significativamente menos eficiente do que chamá-lo fora do contexto (isso faz com que os gradientes sejam registrados na fita, levando a maior uso de CPU e memória). Chame GradientTape.gradient dentro do contexto apenas se você realmente quiser rastrear o gradiente para calcular derivadas de ordem superior.  
```  

**Saída:**  
```  
[[<tf.Tensor: shape=(), dtype=float32, numpy=8.0>,  
  <tf.Tensor: shape=(), dtype=float32, numpy=6.0>],  
 [<tf.Tensor: shape=(), dtype=float32, numpy=6.0>, None]]  
```  

Observe que, ao calcular a derivada de um tensor em relação a uma variável da qual ele não depende, em vez de retornar 0.0, a função `gradient()` retorna `None`.  

E isso é tudo, pessoal! Espero que você tenha gostado deste notebook.