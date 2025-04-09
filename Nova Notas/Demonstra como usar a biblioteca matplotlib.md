---
tags:
  - estudo
  - python
  - AprendizadoMaquina
  - "#matplotlib"
Completo: false
Atualizado: 2025-04-09  15.01
Criado: 2025-04-09  14.53
---
🔖[[Aprendizado de máquina]]


**Ferramentas - matplotlib**

*Este notebook demonstra como usar a biblioteca matplotlib para plotar gráficos bonitos.*

<table align="left">
  <td>
    <a href="https://colab.research.google.com/github/ageron/handson-ml3/blob/main/tools_matplotlib.ipynb" target="_parent"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a>
  </td>
  <td>
    <a target="_blank" href="https://kaggle.com/kernels/welcome?src=https://github.com/ageron/handson-ml3/blob/main/tools_matplotlib.ipynb"><img src="https://kaggle.com/static/images/open-in-kaggle.svg" /></a>
  </td>
</table>

# Índice
 <p><div class="lev1"><a href="#Plotando-seu-primeiro-gráfico"><span class="toc-item-num">1&nbsp;&nbsp;</span>Plotando seu primeiro gráfico</a></div><div class="lev1"><a href="#Estilo-e-cor-da-linha"><span class="toc-item-num">2&nbsp;&nbsp;</span>Estilo e cor da linha</a></div><div class="lev1"><a href="#Salvando-uma-figura"><span class="toc-item-num">3&nbsp;&nbsp;</span>Salvando uma figura</a></div><div class="lev1"><a href="#Subplots"><span class="toc-item-num">4&nbsp;&nbsp;</span>Subplots</a></div><div class="lev1"><a href="#Múltiplas-figuras"><span class="toc-item-num">5&nbsp;&nbsp;</span>Múltiplas figuras</a></div><div class="lev1"><a href="#Máquina-de-estados-do-Pyplot:-implícito-vs-explícito"><span class="toc-item-num">6&nbsp;&nbsp;</span>Máquina de estados do Pyplot: implícito <em>vs</em> explícito</a></div><div class="lev1"><a href="#Pylab-vs-Pyplot-vs-Matplotlib"><span class="toc-item-num">7&nbsp;&nbsp;</span>Pylab <em>vs</em> Pyplot <em>vs</em> Matplotlib</a></div><div class="lev1"><a href="#Desenhando-texto"><span class="toc-item-num">8&nbsp;&nbsp;</span>Desenhando texto</a></div><div class="lev1"><a href="#Legendas"><span class="toc-item-num">9&nbsp;&nbsp;</span>Legendas</a></div><div class="lev1"><a href="#Escalas-não-lineares"><span class="toc-item-num">10&nbsp;&nbsp;</span>Escalas não lineares</a></div><div class="lev1"><a href="#Ticks-e-tickers"><span class="toc-item-num">11&nbsp;&nbsp;</span>Ticks e tickers</a></div><div class="lev1"><a href="#Projeção-polar"><span class="toc-item-num">12&nbsp;&nbsp;</span>Projeção polar</a></div><div class="lev1"><a href="#Projeção-3D"><span class="toc-item-num">13&nbsp;&nbsp;</span>Projeção 3D</a></div><div class="lev1"><a href="#Gráfico-de-dispersão"><span class="toc-item-num">14&nbsp;&nbsp;</span>Gráfico de dispersão</a></div><div class="lev1"><a href="#Linhas"><span class="toc-item-num">15&nbsp;&nbsp;</span>Linhas</a></div><div class="lev1"><a href="#Histogramas"><span class="toc-item-num">16&nbsp;&nbsp;</span>Histogramas</a></div><div class="lev1"><a href="#Imagens"><span class="toc-item-num">17&nbsp;&nbsp;</span>Imagens</a></div><div class="lev1"><a href="#Animações"><span class="toc-item-num">18&nbsp;&nbsp;</span>Animações</a></div><div class="lev1"><a href="#Salvando-animações-em-arquivos-de-vídeo"><span class="toc-item-num">19&nbsp;&nbsp;</span>Salvando animações em arquivos de vídeo</a></div><div class="lev1"><a href="#O-que-vem-a-seguir?"><span class="toc-item-num">20&nbsp;&nbsp;</span>O que vem a seguir?</a></div>

# Plotando seu primeiro gráfico

Primeiro, precisamos importar a biblioteca `matplotlib`.


```python
import matplotlib
```

Agora, vamos plotar nosso primeiro gráfico! :)


```python
import matplotlib.pyplot as plt

plt.plot([1, 2, 4, 9, 5, 3])
plt.show()
```

Sim, é tão simples quanto chamar a função `plot` com alguns dados e depois chamar a função `show`!

**Observação**:

> O Matplotlib pode gerar gráficos usando várias bibliotecas gráficas de backend, como Tk, wxPython, etc. Ao executar Python usando a linha de comando, você pode querer especificar qual backend usar logo após importar o matplotlib e antes de plotar qualquer coisa. Por exemplo, para usar o backend Tk, execute `matplotlib.use("TKAgg")`.
> No entanto, em um notebook Jupyter, as coisas são mais fáceis: importar `import matplotlib.pyplot` automaticamente registra o próprio Jupyter como um backend, então os gráficos aparecem diretamente no notebook. Antes, era necessário executar `%matplotlib inline`, então você ainda verá isso em alguns notebooks, mas não é mais necessário.

Se a função `plot` receber um array de dados, ela o usará como coordenadas no eixo vertical e usará o índice de cada ponto no array como coordenada horizontal.
Você também pode fornecer dois arrays: um para o eixo horizontal `x` e outro para o eixo vertical `y`:


```python
plt.plot([-3, -2, 5, 0], [1, 6, 4, 3])
plt.show()
```

Os eixos correspondem automaticamente à extensão dos dados. Gostaríamos de dar mais espaço ao gráfico, então vamos chamar a função `axis` para alterar a extensão de cada eixo `[xmin, xmax, ymin, ymax]`.


```python
plt.plot([-3, -2, 5, 0], [1, 6, 4, 3])
plt.axis([-4, 6, 0, 7])
plt.show()
```

Agora, vamos plotar uma função matemática. Usamos a função `linspace` do NumPy para criar um array `x` contendo 500 floats variando de -2 a 2, depois criamos um segundo array `y` calculado como o quadrado de `x` (para aprender sobre NumPy, leia o [tutorial do NumPy](tools_numpy.ipynb)).


```python
import numpy as np

x = np.linspace(-2, 2, 500)
y = x**2

plt.plot(x, y)
plt.show()
```

Isso está um pouco seco, vamos adicionar um título, rótulos para os eixos x e y, e desenhar uma grade.


```python
plt.plot(x, y)
plt.title("Função quadrática")
plt.xlabel("x")
plt.ylabel("y = x**2")
plt.grid(True)
plt.show()
```

# Estilo e cor da linha

Por padrão, o matplotlib desenha uma linha entre pontos consecutivos.


```python
plt.plot([0, 100, 100, 0, 0, 100, 50, 0, 100],
         [0, 0, 100, 100, 0, 100, 130, 100, 0])
plt.axis([-10, 110, -10, 140])
plt.show()
```

Você pode passar um terceiro argumento para alterar o estilo e a cor da linha.
Por exemplo, `"g--"` significa "linha tracejada verde".


```python
plt.plot([0, 100, 100, 0, 0, 100, 50, 0, 100],
         [0, 0, 100, 100, 0, 100, 130, 100, 0],
         "g--")
plt.axis([-10, 110, -10, 140])
plt.show()
```

Você pode plotar várias linhas em um único gráfico de forma muito simples: basta passar `x1, y1, [estilo1], x2, y2, [estilo2], ...`.

Por exemplo:


```python
plt.plot([0, 100, 100, 0, 0], [0, 0, 100, 100, 0], "r-",
         [0, 100, 50, 0, 100], [0, 100, 130, 100, 0], "g--")
plt.axis([-10, 110, -10, 140])
plt.show()
```

Ou simplesmente chame `plot` várias vezes antes de chamar `show`.


```python
plt.plot([0, 100, 100, 0, 0], [0, 0, 100, 100, 0], "r-")
plt.plot([0, 100, 50, 0, 100], [0, 100, 130, 100, 0], "g--")
plt.axis([-10, 110, -10, 140])
plt.show()
```

Você também pode desenhar pontos simples em vez de linhas. Aqui está um exemplo com traços verdes, linha pontilhada vermelha e triângulos azuis.
Confira [a documentação](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.plot.html#matplotlib.pyplot.plot) para a lista completa de opções de estilo e cor.


```python
x = np.linspace(-1.4, 1.4, 30)
plt.plot(x, x, 'g--', x, x**2, 'r:', x, x**3, 'b^')
plt.show()
```

A função `plot` retorna uma lista de objetos `Line2D` (um para cada linha). Você pode definir propriedades extras nessas linhas, como a largura da linha, o estilo do traço ou o nível de alpha. Veja a lista completa de propriedades na [documentação](https://matplotlib.org/stable/tutorials/introductory/pyplot.html#controlling-line-properties).


```python
x = np.linspace(-1.4, 1.4, 30)
line1, line2, line3 = plt.plot(x, x, 'g--', x, x**2, 'r:', x, x**3, 'b^')
line1.set_linewidth(3.0)
line1.set_dash_capstyle("round")
line3.set_alpha(0.2)
plt.show()
```

# Salvando uma figura
Salvar uma figura em disco é tão simples quanto chamar [`savefig`](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.savefig.html) com o nome do arquivo (ou um objeto de arquivo). Os formatos de imagem disponíveis dependem do backend gráfico que você usa.


```python
x = np.linspace(-1.4, 1.4, 30)
plt.plot(x, x**2)
plt.savefig("minha_funcao_quadrada.png", transparent=True)
```

# Subplots
Uma figura do matplotlib pode conter vários subplots. Esses subplots são organizados em uma grade. Para criar um subplot, basta chamar a função `subplot` e especificar o número de linhas e colunas na figura e o índice do subplot que você deseja desenhar (começando em 1, da esquerda para a direita e de cima para baixo). Observe que o pyplot mantém o controle do subplot ativo atual (que você pode obter uma referência chamando `plt.gca()`), então quando você chama a função `plot`, ela desenha no subplot *ativo*.
```markdown
# Subplots
(continuação)

```python
plt.subplot2grid((3,3), (2, 0), colspan=2)
plt.plot(x, x**5)
plt.show()
```

Se você precisar de ainda mais flexibilidade no posicionamento de subplots, confira o [tutorial correspondente do matplotlib](https://matplotlib.org/stable/tutorials/intermediate/arranging_axes.html).

# Múltiplas figuras
Também é possível desenhar múltiplas figuras. Cada figura pode conter um ou mais subplots. Por padrão, o matplotlib cria `figure(1)` automaticamente. Quando você alterna entre figuras, o pyplot mantém o controle da figura ativa atual (que você pode obter uma referência chamando `plt.gcf()`), e o subplot ativo dessa figura se torna o subplot atual.

```python
x = np.linspace(-1.4, 1.4, 30)

plt.figure(1)
plt.subplot(211)
plt.plot(x, x**2)
plt.title("Quadrado e Cubo")
plt.subplot(212)
plt.plot(x, x**3)

plt.figure(2, figsize=(10, 5))
plt.subplot(121)
plt.plot(x, x**4)
plt.title("y = x**4")
plt.subplot(122)
plt.plot(x, x**5)
plt.title("y = x**5")

plt.figure(1)      # volta para a figura 1, subplot atual é 212 (inferior)
plt.plot(x, -x**3, "r:")

plt.show()
```

# Máquina de estados do Pyplot: implícito vs explícito
Até agora usamos a máquina de estados do Pyplot que mantém o controle do subplot ativo atual. Toda vez que você chama a função `plot`, o pyplot simplesmente desenha no subplot ativo atual. Ele também faz alguma mágica, como criar automaticamente uma figura e um subplot quando você chama `plot`, se eles ainda não existirem. Essa mágica é conveniente em um ambiente interativo (como o Jupyter).

Mas quando você está escrevendo um programa, *explícito é melhor que implícito*. Código explícito geralmente é mais fácil de depurar e manter, e se você não acredita em mim, basta ler a segunda regra no Zen do Python:

```python
import this
```

Felizmente, o Pyplot permite que você ignore completamente a máquina de estados, então você pode escrever um código lindamente explícito. Basta chamar a função `subplots` e usar o objeto figura e a lista de objetos de eixos que são retornados. Chega de mágica! Por exemplo:

```python
x = np.linspace(-2, 2, 200)
fig1, (ax_top, ax_bottom) = plt.subplots(2, 1, sharex=True)
fig1.set_size_inches(10,5)
line1, line2 = ax_top.plot(x, np.sin(3*x**2), "r-", x, np.cos(5*x**2), "b-")
line3, = ax_bottom.plot(x, np.sin(3*x), "r-")
ax_top.grid(True)

fig2, ax = plt.subplots(1, 1)
ax.plot(x, x**2)
plt.show()
```

Para consistência, continuaremos usando a máquina de estados do pyplot no restante deste tutorial, mas recomendamos usar a interface orientada a objetos em seus programas.

# Pylab vs Pyplot vs Matplotlib

Há alguma confusão sobre a relação entre pylab, pyplot e matplotlib. É simples: matplotlib é a biblioteca completa, ela contém tudo, incluindo pylab e pyplot.

Pyplot fornece várias ferramentas para plotar gráficos, incluindo a interface de máquina de estados para a biblioteca de plotagem orientada a objetos subjacente.

Pylab é um módulo de conveniência que importa matplotlib.pyplot e NumPy em um único namespace. Você encontrará muitos exemplos usando pylab, mas agora é [fortemente desencorajado](https://matplotlib.org/stable/api/index.html#module-pylab) (porque imports *explícitos* são melhores que *implícitos*).

# Desenhando texto
Você pode chamar `text` para adicionar texto em qualquer localização no gráfico. Basta especificar as coordenadas horizontal e vertical e o texto, e opcionalmente alguns argumentos extras. Qualquer texto no matplotlib pode conter expressões de equações TeX, veja [a documentação](https://matplotlib.org/stable/tutorials/text/mathtext.html) para mais detalhes.

```python
x = np.linspace(-1.5, 1.5, 30)
px = 0.8
py = px**2

plt.plot(x, x**2, "b-", px, py, "ro")

plt.text(0, 1.5, "Função quadrática\n$y = x^2$", fontsize=20, color='blue',
         horizontalalignment="center")
plt.text(px - 0.08, py, "Ponto bonito", ha="right", weight="heavy")
plt.text(px + 0.05, py - 0.4, "x = %0.2f\ny = %0.2f"%(px, py), rotation=-30,
         color='gray')

plt.show()
```

* Nota: `ha` é um apelido para `horizontalalignment`

Para mais propriedades de texto, visite [a documentação](https://matplotlib.org/stable/tutorials/text/text_props.html).

De vez em quando é necessário anotar elementos de um gráfico, como o ponto bonito acima. A função `annotate` facilita isso: basta indicar a localização do ponto de interesse e a posição do texto, além de alguns argumentos extras opcionais para o texto e a seta.

```python
plt.plot(x, x**2, px, py, "ro")
plt.annotate("Ponto bonito", xy=(px, py), xytext=(px-1.3,py+0.5),
                           color="green", weight="heavy", fontsize=14,
                           arrowprops={"facecolor": "lightgreen"})
plt.show()
```

Você também pode adicionar uma caixa delimitadora ao redor do seu texto usando o argumento `bbox`:

```python
plt.plot(x, x**2, px, py, "ro")

bbox_props = dict(boxstyle="rarrow,pad=0.3", ec="b", lw=2, fc="lightblue")
plt.text(px-0.2, py, "Ponto bonito", bbox=bbox_props, ha="right")

bbox_props = dict(boxstyle="round4,pad=1,rounding_size=0.2", ec="black",
                  fc="#EEEEFF", lw=5)
plt.text(0, 1.5, "Função quadrática\n$y = x^2$", fontsize=20, color='black',
         ha="center", bbox=bbox_props)

plt.show()
```

Só por diversão, se você quiser um gráfico no estilo [xkcd](https://xkcd.com), basta desenhar dentro de uma seção `with plt.xkcd()`:

```python
with plt.xkcd():
    plt.plot(x, x**2, px, py, "ro")

    bbox_props = dict(boxstyle="rarrow,pad=0.3", ec="b", lw=2, fc="lightblue")
    plt.text(px-0.2, py, "Ponto bonito", bbox=bbox_props, ha="right")

    bbox_props = dict(boxstyle="round4,pad=1,rounding_size=0.2", ec="black",
                      fc="#EEEEFF", lw=5)
    plt.text(0, 1.5, "Função quadrática\n$y = x^2$", fontsize=20, color='black',
             ha="center", bbox=bbox_props)

    plt.show()
```

# Legendas
A maneira mais simples de adicionar uma legenda é definir um rótulo em todas as linhas e depois chamar a função `legend`.

```python
x = np.linspace(-1.4, 1.4, 50)
plt.plot(x, x**2, "r--", label="Função quadrática")
plt.plot(x, x**3, "g-", label="Função cúbica")
plt.legend(loc="best")
plt.grid(True)
plt.show()
```

# Escalas não lineares
O Matplotlib suporta escalas não lineares, como escalas logarítmicas ou logit.

```python
x = np.linspace(0.1, 15, 500)
y = x**3/np.exp(2*x)

plt.figure(1)
plt.plot(x, y)
plt.yscale('linear')
plt.title('linear')
plt.grid(True)

plt.figure(2)
plt.plot(x, y)
plt.yscale('log')
plt.title('log')
plt.grid(True)

plt.figure(3)
plt.plot(x, y)
plt.yscale('logit')
plt.title('logit')
plt.grid(True)

plt.figure(4)
plt.plot(x, y - y.mean())
plt.yscale('symlog', linthresh=0.05)
plt.title('symlog')
plt.grid(True)

plt.show()
```

# Ticks e tickers
Os eixos têm pequenas marcas chamadas "ticks". Para ser preciso, "ticks" são as *localizações* das marcas (por exemplo, (-1, 0, 1)), "tick lines" são as pequenas linhas desenhadas nessas localizações, "tick labels" são os rótulos desenhados ao lado das linhas de tick, e "tickers" são objetos capazes de decidir onde colocar os ticks. Os tickers padrão geralmente fazem um bom trabalho ao colocar ~5 a 8 ticks a uma distância razoável um do outro.

Mas às vezes você precisa de mais controle (por exemplo, há muitos rótulos de tick no gráfico logit acima). Felizmente, o matplotlib oferece controle total sobre os ticks. Você pode até ativar ticks menores.

```python
x = np.linspace(-2, 2, 100)

plt.figure(1, figsize=(15,10))
plt.subplot(131)
plt.plot(x, x**3)
plt.grid(True)
plt.title("Ticks padrão")

ax = plt.subplot(132)
plt.plot(x, x**3)
ax.xaxis.set_ticks(np.arange(-2, 2, 1))
plt.grid(True)
plt.title("Ticks manuais no eixo x")

ax = plt.subplot(133)
plt.plot(x, x**3)
plt.minorticks_on()
ax.tick_params(axis='x', which='minor', bottom=False)
ax.xaxis.set_ticks([-2, 0, 1, 2])
ax.yaxis.set_ticks(np.arange(-5, 5, 1))
ax.yaxis.set_ticklabels(["min", -4, -3, -2, -1, 0, 1, 2, 3, "max"])
plt.grid(True)
plt.title("Ticks e rótulos manuais\n(mais ticks menores) no eixo y")

plt.show()
```

# Projeção polar
Desenhar um gráfico polar é tão fácil quanto definir o argumento `projection` para `"polar"` ao criar o subplot.

```python
radius = 1
theta = np.linspace(0, 2*np.pi*radius, 1000)

plt.subplot(111, projection='polar')
plt.plot(theta, np.sin(5*theta), "g-")
plt.plot(theta, 0.5*np.cos(20*theta), "b-")
plt.show()
```

# Projeção 3D

Plotar gráficos 3D é bastante direto: ao criar um subplot, defina a `projection` para `"3d"`. Isso retorna um objeto de eixos 3D, que você pode usar para chamar `plot_surface`, fornecendo coordenadas x, y e z, além de outros argumentos opcionais. Para mais informações sobre como gerar gráficos 3D, confira o [tutorial do matplotlib](https://matplotlib.org/stable/tutorials/toolkits/mplot3d.html).

```python
x = np.linspace(-5, 5, 50)
y = np.linspace(-5, 5, 50)
X, Y = np.meshgrid(x, y)
R = np.sqrt(X**2 + Y**2)
Z = np.sin(R)

figure = plt.figure(1, figsize = (12, 4))
subplot3d = plt.subplot(111, projection='3d')
surface = subplot3d.plot_surface(X, Y, Z, rstride=1, cstride=1,
                                 cmap=matplotlib.cm.coolwarm, linewidth=0.1)
plt.show()
```

Outra maneira de exibir esses mesmos dados é através de um gráfico de contorno.

```python
plt.contourf(X, Y, Z, cmap=matplotlib.cm.coolwarm)
plt.colorbar()
plt.show()
```

# Gráfico de dispersão

Para desenhar um gráfico de dispersão, basta fornecer as coordenadas x e y dos pontos.

```python
np.random.seed(42)  # para tornar a saída deste notebook reproduzível

x, y = np.random.rand(2, 100)
plt.scatter(x, y)
plt.show()
```

Você também pode opcionalmente fornecer a escala de cada ponto.

```python
x, y, scale = np.random.rand(3, 100)
scale = 500 * scale ** 5
plt.scatter(x, y, s=scale)
plt.show()
```

E como de costume, há vários outros argumentos que você pode fornecer, como as cores de preenchimento e borda e o nível alpha.

```python
for color in ['red', 'green', 'blue']:
    n = 100
    x, y = np.random.rand(2, n)
    scale = 500.0 * np.random.rand(n) ** 5
    plt.scatter(x, y, s=scale, c=color, alpha=0.3, edgecolors='blue')

plt.grid(True)

plt.show()
```

# Linhas
Você pode desenhar linhas simplesmente usando a função `plot`, como fizemos até agora. No entanto, muitas vezes é conveniente criar uma função utilitária que plota uma linha (aparentemente) infinita através do gráfico, dada uma inclinação e um intercepto. Você também pode usar as funções `hlines` e `vlines` que plotam segmentos de linha horizontais e verticais.
Por exemplo:

```python
def plot_line(axis, slope, intercept, **kwargs):
    xmin, xmax = axis.get_xlim()
    plt.plot([xmin, xmax],
             [xmin*slope+intercept, xmax*slope+intercept],
             **kwargs)

x = np.random.randn(1000)
y = 0.5*x + 5 + np.random.randn(1000) * 2
plt.axis([-2.5, 2.5, -5, 15])
plt.scatter(x, y, alpha=0.2)
plt.plot(1, 0, "ro")
plt.vlines(1, -5, 0, color="red")
plt.hlines(0, -2.5, 1, color="red")
plot_line(axis=plt.gca(), slope=0.5, intercept=5, color="magenta")
plt.grid(True)
plt.show()
```

# Histogramas

```python
data = [1, 1.1, 1.8, 2, 2.1, 3.2, 3, 3, 3, 3]
plt.subplot(211)
plt.hist(data, bins = 10, rwidth=0.8)

plt.subplot(212)
plt.hist(data, bins = [1, 1.5, 2, 2.5, 3], rwidth=0.95)
plt.xlabel("Valor")
plt.ylabel("Frequência")

plt.show()
```

```python
data1 = np.random.randn(400)
data2 = np.random.randn(500) + 3
data3 = np.random.randn(450) + 6
data4a = np.random.randn(200) + 9
data4b = np.random.randn(100) + 10

plt.hist(data1, bins=5, color='g', alpha=0.75, histtype='bar',  # tipo padrão
         label='histograma de barras')
plt.hist(data2, color='b', alpha=0.65, histtype='stepfilled',
         label='histograma preenchido')
plt.hist(data3, color='r', histtype='step', label='histograma de passos')
plt.hist((data4a, data4b), color=('r','m'), alpha=0.55, histtype='barstacked',
         label=('empilhado a', 'empilhado b'))

plt.xlabel("Valor")
plt.ylabel("Frequência")
plt.legend()
plt.grid(True)
plt.show()
```

# Imagens
Ler, gerar e plotar imagens no matplotlib é bastante direto.

Para ler uma imagem, basta importar o módulo `matplotlib.image` e chamar sua função `imread`, passando o nome do arquivo (ou objeto de arquivo). Ela retorna os dados da imagem, como um array NumPy. Vamos tentar com a imagem `my_square_function.png` que salvamos anteriormente.

```python
import matplotlib.image as mpimg

img = mpimg.imread("my_square_function.png")
print(img.shape, img.dtype)
```

Carregamos uma imagem 288x432. Cada pixel é representado por um array de 4 elementos: níveis vermelho, verde, azul e alpha, armazenados como floats de 32 bits entre 0 e 1. Agora tudo o que precisamos fazer é chamar `imshow`:

```python
plt.imshow(img)
plt.show()
```

Tadaaa! Você pode querer ocultar os eixos ao exibir uma imagem:

```python
plt.imshow(img)
plt.axis('off')
plt.show()
```

Nos bastidores, `imread()` usa a Python Image Library (PIL), e a documentação do Matplotlib agora recomenda usar o PIL diretamente:

```python
import PIL

img = np.asarray(PIL.Image.open("my_square_function.png"))
print(img.shape, img.dtype)
```

Observe que o array agora contém inteiros não assinados de 8 bits (de 0 a 255), mas isso é bom, pois `plt.imshow()` também suporta esse formato:

```python
plt.imshow(img)
plt.axis('off')
plt.show()
```

Você também pode gerar uma imagem do zero:

```python
img = np.arange(100*100).reshape(100, 100)
print(img)
plt.imshow(img)
plt.show()
```

Como não fornecemos níveis RGB, a função `imshow` mapeia automaticamente os valores para um gradiente de cores. Por padrão, o gradiente de cores vai do azul (para valores baixos) ao amarelo (para valores altos), mas você pode selecionar outro mapa de cores. Por exemplo:

```python
plt.imshow(img, cmap="hot")
plt.show()
```

Você também pode gerar uma imagem RGB diretamente:

```python
img = np.empty((20,30,3))
img[:, :10] = [0, 0, 0.6]
img[:, 10:20] = [1, 1, 1]
img[:, 20:] = [0.6, 0, 0]
plt.imshow(img)
plt.show()
```

Como o array `img` é muito pequeno (20x30), quando a função `imshow` o exibe, ela amplia a imagem para o tamanho da figura. Imagine esticar a imagem original, deixando espaços em branco entre os pixels originais. Como o imshow preenche os espaços em branco? Bem, por padrão, ele apenas colore cada pixel em branco usando a cor do pixel não branco mais próximo. Essa técnica pode levar a imagens pixeladas. Se preferir, você pode usar um método de interpolação diferente, como [interpolação bilinear](https://pt.wikipedia.org/wiki/Interpola%C3%A7%C3%A3o_bilinear) para preencher os pixels em branco. Isso resulta em bordas desfocadas, o que pode ser mais agradável em alguns casos:

```python
plt.imshow(img, interpolation="bilinear")
plt.show()
```

# Animações
Embora o matplotlib seja usado principalmente para gerar imagens, ele também é capaz de exibir animações. Primeiro, você precisa importar `matplotlib.animation`.

```python
import matplotlib.animation as animation
```

No exemplo a seguir, começamos criando pontos de dados, depois criamos um gráfico vazio, definimos a função de atualização que será chamada a cada iteração da animação e finalmente adicionamos uma animação ao gráfico criando uma instância `FuncAnimation`.

O construtor `FuncAnimation` recebe uma figura, uma função de atualização e argumentos opcionais. Especificamos que queremos uma animação com 50 quadros, com 100ms entre cada quadro. A cada iteração, `FuncAnimation` chama nossa função de atualização e passa o número do quadro `num` (de 0 a 49 no nosso caso) seguido pelos argumentos extras que especificamos com `fargs`.

Nossa função de atualização simplesmente define os dados da linha para serem os primeiros `num` pontos de dados (para que os dados sejam desenhados gradualmente) e apenas por diversão também adicionamos um pequeno número aleatório a cada ponto de dados para que a linha pareça oscilar.

```python
x = np.linspace(-1, 1, 100)
y = np.sin(x**2*25)
data = np.array([x, y])

fig = plt.figure()
line, = plt.plot([], [], "r-")  # começa com um gráfico vazio
plt.axis([-1.1, 1.1, -1.1, 1.1])
plt.plot([-0.5, 0.5], [0, 0], "b-", [0, 0], [-0.5, 0.5], "b-", 0, 0, "ro")
plt.grid(True)
plt.title("Animação maravilhosa")

# esta função será chamada a cada iteração
def update_line(num, data, line):
    # plotamos apenas os primeiros `num` pontos de dados.
    line.set_data(data[..., :num] + np.random.rand(2, num) / 25)
    return line,

line_ani = animation.FuncAnimation(fig, update_line, frames=50,
                                   fargs=(data, line), interval=100)
plt.close()  # chame close() para evitar exibir o gráfico estático
```

Em seguida, vamos exibir a animação. Uma opção é convertê-la para código HTML5 (usando uma tag `<video>`), e renderizar esse código usando `IPython.display.HTML`:

```python
from IPython.display import HTML

HTML(line_ani.to_html5_video())
```

Alternativamente, podemos exibir a animação usando um pequeno widget interativo HTML/Javascript:

```python
HTML(line_ani.to_jshtml())
```

Você pode configurar o Matplotlib para usar esse widget por padrão ao renderizar animações:

```python
matplotlib.rc('animation', html='jshtml')
```

Depois disso, você nem precisa mais usar `IPython.display.HTML`:

```python
animation.FuncAnimation(fig, update_line, frames=50, fargs=(data, line),
                        interval=100)
```

**Aviso:** se você salvar o notebook junto com suas saídas, as animações ocuparão muito espaço.

# Salvando animações em arquivos de vídeo
O Matplotlib depende de bibliotecas de terceiros para escrever vídeos, como [FFMPEG](https://www.ffmpeg.org/) ou [ImageMagick](https://imagemagick.org/). No exemplo a seguir, usaremos o FFMPEG, então certifique-se de instalá-lo primeiro. Para salvar a animação no formato GIF, você precisaria do ImageMagick.

```python
Writer = animation.writers['ffmpeg']
writer = Writer(fps=15, metadata=dict(artist='Me'), bitrate=1800)
line_ani.save('minha_animacao_oscilante.mp4', writer=writer)
```

# O que vem a seguir?
Agora você conhece todos os conceitos básicos do matplotlib, mas há muitas outras opções disponíveis. A melhor maneira de aprender mais é visitar a [galeria](https://matplotlib.org/stable/gallery/index.html), observar as imagens, escolher um gráfico que lhe interesse, copiar o código em um notebook Jupyter e experimentar.
