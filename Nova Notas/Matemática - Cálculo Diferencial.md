---
Atualizado: 2025-03-14  10.50
Criado: 2025-03-13  18.13
---
**Matemática - Cálculo Diferencial**

O cálculo é o estudo da mudança contínua. Ele possui dois principais subcampos: *cálculo diferencial*, que estuda a taxa de mudança das funções, e *cálculo integral*, que estuda a área sob a curva. Neste notebook, discutiremos o primeiro.

*O cálculo diferencial está no cerne do Aprendizado Profundo (Deep Learning), por isso é importante entender o que são derivadas e gradientes, como eles são usados no Aprendizado Profundo e compreender quais são suas limitações.*

**Nota:** o código neste notebook é usado apenas para criar figuras e animações. Você não precisa entender como ele funciona (embora eu tenha feito o meu melhor para deixá-lo claro, caso você esteja interessado).

# Inclinação de uma linha reta

```run-python
#@title
import matplotlib as mpl
import matplotlib.pyplot as plt
import numpy as np

# Para obter animações suaves
import matplotlib.animation as animation
mpl.rc('animation', html='jshtml')
```

Qual é a inclinação da seguinte linha?

```run-python
#@title
def get_AB_line(A_pos, B_pos, x_min=-1000, x_max=+1000):
    rise = B_pos[1] - A_pos[1]
    run = B_pos[0] - A_pos[0]
    slope = rise / run
    offset = A_pos[1] - slope * A_pos[0]
    return [x_min, x_max], [x_min * slope + offset, x_max * slope + offset]

def plot_AB_line(A_pos, B_pos, A_name="A", B_name="B"):
    for point, name in ((A_pos, A_name), (B_pos, B_name)):
        plt.plot(point[0], point[1], "bo")
        plt.text(point[0] - 0.35, point[1], name, fontsize=14)
    xs, ys = get_AB_line(A_pos, B_pos)
    plt.plot(xs, ys)

def plot_rise_over_run(A_pos, B_pos):
    plt.plot([A_pos[0], B_pos[0]], [A_pos[1], A_pos[1]], "k--")
    plt.text((A_pos[0] + B_pos[0]) / 2, A_pos[1] - 0.4, "run", fontsize=14)
    plt.plot([B_pos[0], B_pos[0]], [A_pos[1], B_pos[1]], "k--")
    plt.text(B_pos[0] + 0.2, (A_pos[1] + B_pos[1]) / 2, "rise", fontsize=14)

def show(axis="equal", ax=None, title=None, xlabel="$x$", ylabel="$y$"):
    ax = ax or plt.gca()
    ax.axis(axis)
    ax.grid()
    ax.set_title(title, fontsize=14)
    ax.set_xlabel(xlabel, fontsize=14)
    ax.set_ylabel(ylabel, fontsize=14, rotation=0)
    ax.axhline(y=0, color='k')
    ax.axvline(x=0, color='k')

A_pos = np.array([1, 1])
B_pos = np.array([7, 4])
plot_AB_line(A_pos, B_pos)
plot_rise_over_run(A_pos, B_pos)
show([0, 8.4, 0, 5.5], title="Slope = rise / run")
```

Como você provavelmente sabe, a inclinação de uma linha reta (não vertical) pode ser calculada tomando dois pontos $\mathrm{A}$ e $\mathrm{B}$ na linha e calculando a "elevação sobre o deslocamento":

$slope = \dfrac{rise}{run} = \dfrac{\Delta y}{\Delta x} = \dfrac{y_\mathrm{B} - y_\mathrm{A}}{x_\mathrm{B} - x_\mathrm{A}}$

Neste exemplo, a elevação é 3 e o deslocamento é 6, então a inclinação é 3/6 = 0.5.

# Definindo a inclinação de uma curva

Mas e se você quiser saber a inclinação de algo que não seja uma linha reta? Por exemplo, vamos considerar a curva definida por $y = f(x) = x^2$:

```run-python
#@title
xs = np.linspace(-2.1, 2.1, 500)
ys = xs**2
plt.plot(xs, ys)

plt.plot([0, 0], [0, 3], "k--")
plt.arrow(-1.4, 2.5, 0.5, -1.3, head_width=0.1)
plt.arrow(0.85, 1.05, 0.5, 1.3, head_width=0.1)
show([-2.1, 2.1, 0, 2.8], title="Slope of the curve $y = x^2$")
```

Obviamente, a inclinação varia: à esquerda (ou seja, quando $x<0$), a inclinação é negativa (ou seja, quando nos movemos da esquerda para a direita, a curva desce), enquanto à direita (ou seja, quando $x>0$), a inclinação é positiva (ou seja, quando nos movemos da esquerda para a direita, a curva sobe). No ponto $x=0$, a inclinação é igual a 0 (ou seja, a curva é localmente plana). O fato de a inclinação ser 0 quando atingimos um mínimo (ou de fato um máximo) é crucialmente importante, e voltaremos a isso mais tarde.

Como podemos colocar números nessas intuições? Bem, digamos que queremos estimar a inclinação da curva em um ponto $\mathrm{A}$. Podemos fazer isso tomando outro ponto $\mathrm{B}$ na curva, não muito distante, e então calculando a inclinação entre esses dois pontos:

```python
#@title
def animate_AB_line(f, fp, f_str, x_A, axis=None):
    y_A = f(x_A)
    eps = 1e-4
    x_B_range = 1.5
    x_B = x_A + eps

    n_frames = 200
    text_offset_A = -0.2
    text_offset_B = +0.1
    x_min, x_max = -1000, 1000

    fig, ax = plt.subplots()

    # plot f(x)
    xs = np.linspace(-2.1, 2.1, 500)
    ys = f(xs)
    ax.plot(xs, ys)

    # plot the tangent to the curve at point A
    if fp:
        slope = fp(x_A)
        offset = y_A - slope * x_A
        ax.plot([x_min, x_max], [slope*x_min + offset, slope*x_max + offset],
                "y--")

    # plot the line AB and the labels A and B so they can be animated
    y_A = f(x_A)
    y_B = f(x_B)
    xs, ys = get_AB_line([x_A, y_A], [x_B, y_B])
    line_inf, = ax.plot(xs, ys, "-")
    line_AB, = ax.plot([x_A, x_B], [y_A, y_B], "bo-")
    ax.text(x_A + text_offset_A, y_A, "A", fontsize=14)
    B_text = ax.text(x_B + text_offset_B, y_B, "B", fontsize=14)

    # plot the grid and axis labels
    title = r"Slope of the curve $y = {}$ at $x_\mathrm{{A}} = {}$".format(f_str, x_A)
    show(axis or [-2.1, 2.1, 0, 2.8], title=title)

    def update_graph(i):
        x_B = x_A + x_B_range * np.cos(i * 2 * np.pi / n_frames) ** 3
        if np.abs(x_B - x_A) < eps:
            x_B = x_A + eps # to avoid division by 0
        y_B = f(x_B)
        xs, ys = get_AB_line([x_A, y_A], [x_B, y_B])
        line_inf.set_data(xs, ys)
        line_AB.set_data([x_A, x_B], [y_A, y_B])
        B_text.set_position([x_B + text_offset_B, y_B])
        return line_inf, line_AB

    anim = animation.FuncAnimation(fig, update_graph,
                                  init_func=lambda: update_graph(0),
                                  frames=n_frames,
                                  interval=20,
                                  blit=True)
    plt.close()
    return anim

animate_AB_line(lambda x: x**2, lambda x: 2*x, "x^2", -1)
```

Como você pode ver, quando o ponto $\mathrm{B}$ está muito próximo do ponto $\mathrm{A}$, a linha $(\mathrm{AB})$ se torna quase indistinguível da própria curva (pelo menos localmente ao redor do ponto $\mathrm{A}$). A linha $(\mathrm{AB})$ se aproxima cada vez mais da **linha tangente** à curva no ponto $\mathrm{A}$: esta é a melhor aproximação linear da curva no ponto $\mathrm{A}$.

Portanto, faz sentido definir a inclinação da curva no ponto $\mathrm{A}$ como a inclinação que a linha $\mathrm{(AB)}$ se aproxima quando $\mathrm{B}$ se aproxima infinitamente de $\mathrm{A}$. Essa inclinação é chamada de **derivada** da função $f$ em $x=x_\mathrm{A}$. Por exemplo, a derivada da função $f(x)=x^2$ em $x=x_\mathrm{A}$ é igual a $2x_\mathrm{A}$ (veremos como obter esse resultado em breve), então no gráfico acima, como o ponto $\mathrm{A}$ está localizado em $x_\mathrm{A}=-1$, a linha tangente à curva nesse ponto tem uma inclinação de $-2$.

# Diferenciabilidade

Observe que algumas funções não são tão bem comportadas quanto $x^2$. Por exemplo, considere a função $f(x)=|x|$, o valor absoluto de $x$:

```python
#@title
animate_AB_line(lambda x: np.abs(x), None, "|x|", 0)
```

Não importa o quanto você dê zoom na origem (o ponto em $x=0, y=0$), a curva sempre parecerá um V. A inclinação é -1 para qualquer $x < 0$ e +1 para qualquer $x > 0$, mas **em $x = 0$, a inclinação é indefinida**, pois não é possível aproximar a curva $y=|x|$ localmente ao redor da origem usando uma linha reta, não importa o quanto você dê zoom nesse ponto.

A função $f(x)=|x|$ é dita **não diferenciável** em $x=0$: sua derivada é indefinida em $x=0$. Isso significa que a curva $y=|x|$ tem uma inclinação indefinida nesse ponto. No entanto, a função $f(x)=|x|$ é **diferenciável** em todos os outros pontos.

Para que uma função $f(x)$ seja diferenciável em algum ponto $x_\mathrm{A}$, a inclinação da linha $(\mathrm{AB})$ deve se aproximar de um único valor finito à medida que $\mathrm{B}$ se aproxima infinitamente de $\mathrm{A}$.

Isso implica várias restrições:

* Primeiro, a função deve, é claro, ser **definida** em $x_\mathrm{A}$. Como contraexemplo, a função $f(x)=\dfrac{1}{x}$ é indefinida em $x_\mathrm{A}=0$, portanto, não é diferenciável nesse ponto.
* A função também deve ser **contínua** em $x_\mathrm{A}$, o que significa que, à medida que $x_\mathrm{B}$ se aproxima infinitamente de $x_\mathrm{A}$, $f(x_\mathrm{B})$ também deve se aproximar infinitamente de $f(x_\mathrm{A})$. Como contraexemplo, $f(x)=\begin{cases}-1 \text{ se }x < 0\\+1 \text{ se }x \geq 0\end{cases}$ não é contínua em $x_\mathrm{A}=0$, embora seja definida nesse ponto: de fato, quando você se aproxima dela pelo lado negativo, ela não se aproxima infinitamente de $f(0)=+1$. Portanto, não é contínua nesse ponto e, portanto, também não é diferenciável.
* A função não deve ter um **ponto de quebra** em $x_\mathrm{A}$, o que significa que a inclinação que a linha $(\mathrm{AB})$ se aproxima à medida que $\mathrm{B}$ se aproxima de $\mathrm{A}$ deve ser a mesma, independentemente de $\mathrm{B}$ se aproximar pelo lado esquerdo ou pelo lado direito. Já vimos um contraexemplo com $f(x)=|x|$, que é definida e contínua em $x_\mathrm{A}=0$, mas tem um ponto de quebra em $x_\mathrm{A}=0$: a inclinação da curva $y=|x|$ é -1 à esquerda e +1 à direita.
* A curva $y=f(x)$ não deve ser **vertical** no ponto $\mathrm{A}$. Um contraexemplo é $f(x)=\sqrt[3]{x}$, a raiz cúbica de $x$: a curva é vertical na origem, então a função não é diferenciável em $x_\mathrm{A}=0$, como você pode ver na seguinte animação:

```python
#@title
animate_AB_line(lambda x: np.cbrt(x), None, r"\sqrt[3]{x}", 0,
                axis=[-2.1, 2.1, -1.4, 1.4])
```

Agora vamos ver como realmente diferenciar uma função (ou seja, encontrar sua derivada).

# Diferenciando uma função

A discussão anterior leva à seguinte definição:

<hr />

A **derivada** de uma função $f(x)$ em $x = x_\mathrm{A}$ é denotada por $f'(x_\mathrm{A})$ e é definida como:

$f'(x_\mathrm{A}) = \underset{x_\mathrm{B} \to x_\mathrm{A}}\lim\dfrac{f(x_\mathrm{B}) - f(x_\mathrm{A})}{x_\mathrm{B} - x_\mathrm{A}}$

<hr />

Não se assuste, isso é mais simples do que parece! Você pode reconhecer a equação _elevação sobre deslocamento_ $\dfrac{y_\mathrm{B} - y_\mathrm{A}}{x_\mathrm{B} - x_\mathrm{A}}$ que discutimos anteriormente. Essa é apenas a inclinação da linha $\mathrm{(AB)}$. E a notação $\underset{x_\mathrm{B} \to x_\mathrm{A}}\lim$ significa que estamos fazendo $x_\mathrm{B}$ se aproximar infinitamente de $x_\mathrm{A}$. Então, em linguagem simples, $f'(x_\mathrm{A})$ é o valor que a inclinação da linha $\mathrm{(AB)}$ se aproxima quando $\mathrm{B}$ se aproxima infinitamente de $\mathrm{A}$. Esta é apenas uma maneira formal de dizer exatamente a mesma coisa que antes.

## Exemplo: encontrando a derivada de $x^2$

Vamos ver um exemplo concreto. Vamos ver se conseguimos determinar qual é a inclinação da curva $y=x^2$ em qualquer ponto $\mathrm{A}$ (tente entender cada linha, prometo que não é tão difícil):

$$
$
\begin{align*}
f'(x_\mathrm{A}) \, & = \underset{x_\mathrm{B} \to x_\mathrm{A}}\lim\dfrac{f(x_\mathrm{B}) - f(x_\mathrm{A})}{x_\mathrm{B} - x_\mathrm{A}} \\
& = \underset{x_\mathrm{B} \to x_\mathrm{A}}\lim\dfrac{{x_\mathrm{B}}^2 - {x_\mathrm{A}}^2}{x_\mathrm{B} - x_\mathrm{A}} \quad && \text{pois } f(x) = x^2\\
& = \underset{x_\mathrm{B} \to x_\mathrm{A}}\lim\dfrac{(x_\mathrm{B} - x_\mathrm{A})(x_\mathrm{B} + x_\mathrm{A})}{x_\mathrm{B} - x_\mathrm{A}}\quad && \text{pois } {x_\mathrm{A}}^2 - {x_\mathrm{B}}^2 = (x_\mathrm{A}-x_\mathrm{B})(x_\mathrm{A}+x_\mathrm{B})\\
& = \underset{x_\mathrm{B} \to x_\mathrm{A}}\lim(x_\mathrm{B} + x_\mathrm{A})\quad && \text{pois os dois } (x_\mathrm{B} - x_\mathrm{A}) \text{ se cancelam}\\
& = \underset{x_\mathrm{B} \to x_\mathrm{A}}\lim x_\mathrm{B} \, + \underset{x_\mathrm{B} \to x_\mathrm{A}}\lim x_\mathrm{A}\quad && \text{pois o limite de uma soma é a soma dos limites}\\
& = x_\mathrm{A} \, + \underset{x_\mathrm{B} \to x_\mathrm{A}}\lim x_\mathrm{A} \quad && \text{pois } x_\mathrm{B}\text{ se aproxima de } x_\mathrm{A} \\
& = x_\mathrm{A} + x_\mathrm{A} \quad && \text{pois } x_\mathrm{A} \text{ permanece constante quando } x_\mathrm{B}\text{ se aproxima de } x_\mathrm{A} \\
& = 2x_\mathrm{A} &&
\end{align*}
$
$$

É isso! Acabamos de provar que a inclinação de $y = x^2$ em qualquer ponto $\mathrm{A}$ é $f'(x_\mathrm{A}) = 2x_\mathrm{A}$. O que fizemos é chamado de **diferenciação**: encontrar a derivada de uma função.

Observe que usamos algumas propriedades importantes dos limites. Aqui estão as principais propriedades que você precisa conhecer para trabalhar com derivadas:

* $\underset{x \to k}\lim c = c \quad$ se $c$ for algum valor constante que não depende de $x$, então o limite é apenas $c$.
* $\underset{x \to k}\lim x = k \quad$ se $x$ se aproximar de algum valor $k$, então o limite é $k$.
* $\underset{x \to k}\lim\,\left[f(x) + g(x)\right] = \underset{x \to k}\lim f(x) + \underset{x \to k}\lim g(x) \quad$ o limite de uma soma é a soma dos limites
* $\underset{x \to k}\lim\,\left[f(x) \times g(x)\right] = \underset{x \to k}\lim f(x) \times \underset{x \to k}\lim g(x) \quad$ o limite de um produto é o produto dos limites

**Nota importante:** no Aprendizado Profundo, a diferenciação é quase sempre realizada automaticamente pela estrutura que você está usando (como TensorFlow ou PyTorch). Isso é chamado de auto-diff, e eu fiz [outro notebook](https://github.com/ageron/handson-ml3/blob/main/extra_autodiff.ipynb) sobre esse tópico. No entanto, você ainda deve garantir que tem uma boa compreensão das derivadas, ou elas podem voltar para assombrá-lo um dia, por exemplo, quando você usar uma raiz quadrada em sua função de custo sem perceber que sua derivada se aproxima do infinito quando $x$ se aproxima de 0 (dica: você deve usar $\sqrt{x+\epsilon}$ em vez disso, onde $\epsilon$ é alguma constante pequena, como $10^{-4}$).

Você frequentemente encontrará uma definição ligeiramente diferente (mas equivalente) da derivada. Vamos derivá-la da definição anterior. Primeiro, vamos definir $\epsilon = x_\mathrm{B} - x_\mathrm{A}$. Em seguida, observe que $\epsilon$ se aproximará de 0 à medida que $x_\mathrm{B}$ se aproxima de $x_\mathrm{A}$. Por último, observe que $x_\mathrm{B} = x_\mathrm{A} + \epsilon$. Com isso, podemos reformular a definição acima da seguinte forma:

$f'(x_\mathrm{A}) = \underset{\epsilon \to 0}\lim\dfrac{f(x_\mathrm{A} + \epsilon) - f(x_\mathrm{A})}{\epsilon}$

Enquanto estamos nisso, vamos renomear $x_\mathrm{A}$ para $x$, para nos livrarmos do subscrito A e tornar a equação mais simples de ler:

<hr />

$f'(x) = \underset{\epsilon \to 0}\lim\dfrac{f(x + \epsilon) - f(x)}{\epsilon}$

<hr />

Ok! Agora vamos usar essa nova definição para encontrar a derivada de $f(x) = x^2$ em qualquer ponto $x$, e (esperançosamente) devemos encontrar o mesmo resultado de antes (exceto usando $x$ em vez de $x_\mathrm{A}$):

$
\begin{align*}
f'(x) \, & = \underset{\epsilon \to 0}\lim\dfrac{f(x + \epsilon) - f(x)}{\epsilon} \\
& = \underset{\epsilon \to 0}\lim\dfrac{(x + \epsilon)^2 - {x}^2}{\epsilon} \quad && \text{pois } f(x) = x^2\\
& = \underset{\epsilon \to 0}\lim\dfrac{{x}^2 + 2x\epsilon + \epsilon^2 - {x}^2}{\epsilon}\quad && \text{pois } (x + \epsilon)^2 = {x}^2 + 2x\epsilon + \epsilon^2\\
& = \underset{\epsilon \to 0}\lim\dfrac{2x\epsilon + \epsilon^2}{\epsilon}\quad && \text{pois os dois } {x}^2 \text{ se cancelam}\\
& = \underset{\epsilon \to 0}\lim \, (2x + \epsilon)\quad && \text{pois } 2x\epsilon \text{ e } \epsilon^2 \text{ podem ambos ser divididos por } \epsilon\\
& = 2x &&
\end{align*}
$

Sim! Funciona.

## Notações

Uma palavra sobre notações: existem várias outras notações para a derivada que você encontrará na literatura:

$f'(x) = \dfrac{\mathrm{d}f(x)}{\mathrm{d}x} = \dfrac{\mathrm{d}}{\mathrm{d}x}f(x)$

Essa notação também é útil quando uma função não tem nome. Por exemplo, $\dfrac{\mathrm{d}}{\mathrm{d}x}[x^2]$ refere-se à derivada da função $x \mapsto x^2$.

Além disso, quando as pessoas falam sobre a função $f(x)$, às vezes omitem "$(x)$" e falam apenas sobre a função $f$. Quando esse é o caso, a notação da derivada também é mais simples:

$f' = \dfrac{\mathrm{d}f}{\mathrm{d}x} = \dfrac{\mathrm{d}}{\mathrm{d}x}f$

A notação $f'$ é a notação de Lagrange, enquanto $\dfrac{\mathrm{d}f}{\mathrm{d}x}$ é a notação de Leibniz.

Existem outras notações menos comuns, como a notação de Newton $\dot y$ (assumindo $y = f(x)$) ou a notação de Euler $\mathrm{D}f$.

## Traçando a tangente a uma curva

Vamos usar a equação $f'(x) = 2x$ para traçar a tangente à curva $y=x^2$ em vários valores de $x$ (você pode clicar no botão de reprodução sob os gráficos para reproduzir a animação):

```python
#@title
def animate_tangent(f, fp, f_str):
    n_frames = 200
    x_min, x_max = -1000, 1000

    fig, (ax1, ax2) = plt.subplots(2, 1, figsize=(5, 8), sharex=True)

    # plot f
    xs = np.linspace(-2.1, 2.1, 500)
    ys = f(xs)
    ax1.plot(xs, ys)

    # plot tangent
    line_tangent, = ax1.plot([x_min, x_max], [0, 0])

    # plot f'
    xs = np.linspace(-2.1, 2.1, 500)
    ys = fp(xs)
    ax2.plot(xs, ys, "r-")

    # plot points A
    point_A1, = ax1.plot(0, 0, "bo")
    point_A2, = ax2.plot(0, 0, "bo")

    show([-2.1, 2.1, 0, 2.8], ax=ax1, ylabel="$f(x)$",
        title=r"$y=f(x)=" + f_str + "$ and the tangent at $x=x_\mathrm{A}$")
    show([-2.1, 2.1, -4.2, 4.2], ax=ax2, ylabel="$f'(x)$",
        title=r"y=f'(x) and the slope of the tangent at $x=x_\mathrm{A}$")

    def update_graph(i):
        x = 1.5 * np.sin(2 * np.pi * i / n_frames)
        f_x = f(x)
        df_dx = fp(x)
        offset = f_x - df_dx * x
        line_tangent.set_data([x_min, x_max],
                              [df_dx * x_min + offset, df_dx * x_max + offset])
        point_A1.set_data(x, f_x)
        point_A2.set_data(x, df_dx)
        return line_tangent, point_A1, point_A2

    anim = animation.FuncAnimation(fig, update_graph,
                                  init_func=lambda: update_graph(0),
                                  frames=n_frames,
                                  interval=20,
                                  blit=True)
    plt.close()
    return anim

def f(x):
  return x**2

def fp(x):
  return 2*x

animate_tangent(f, fp, "x^2")
```

<hr />

**Nota:** considere a linha tangente à curva $y=f(x)$ em algum ponto $\mathrm{A}$. Qual é sua equação? Bem, como a tangente é uma linha reta, sua equação deve ser da forma:

$y = \alpha x + \beta$

onde $\alpha$ é a inclinação da linha e $\beta$ é o deslocamento (ou seja, a coordenada $y$ do ponto em que a linha cruza o eixo vertical). Já sabemos que a inclinação da linha tangente no ponto $\mathrm{A}$ é a derivada de $f(x)$ nesse ponto, então:

$\alpha = f'(x_\mathrm{A})$

Mas e o deslocamento $\beta$? Bem, também sabemos que a linha tangente toca a curva no ponto $\mathrm{A}$, então sabemos que $\alpha x_\mathrm{A} + \beta = f(x_\mathrm{A})$. Então:

$\beta = f(x_\mathrm{A}) - f'(x_\mathrm{A})x_\mathrm{A}$

Portanto, obtemos a seguinte equação para a tangente:

$y = f(x_\mathrm{A}) + f'(x_\mathrm{A})(x - x_\mathrm{A})$

Por exemplo, a tangente à curva $y=x^2$ é dada por:

$y = {x_\mathrm{A}}^2 + 2x_\mathrm{A}(x - x_\mathrm{A}) = 2x_\mathrm{A}x - x_\mathrm{A}^2$
<hr />

# Regras de diferenciação

Uma regra muito importante é que **a derivada de uma soma é a soma das derivadas**. Mais precisamente, se definirmos $f(x) = g(x) + h(x)$, então $f'(x) = g'(x) + h'(x)$. Isso é bastante fácil de provar:

$
\begin{align*}
f'(x) & = \underset{\epsilon \to 0}\lim\dfrac{f(x+\epsilon) - f(x)}{\epsilon} && \quad\text{por definição}\\
& = \underset{\epsilon \to 0}\lim\dfrac{g(x+\epsilon) + h(x+\epsilon) - g(x) - h(x)}{\epsilon} && \quad \text{usando }f(x) = g(x) + h(x) \\
& = \underset{\epsilon \to 0}\lim\dfrac{g(x+\epsilon) - g(x) + h(x+\epsilon) - h(x)}{\epsilon} && \quad \text{apenas reorganizando os termos}\\
& = \underset{\epsilon \to 0}\lim\dfrac{g(x+\epsilon) - g(x)}{\epsilon} + \underset{\epsilon \to 0}\lim\dfrac{h(x+\epsilon) - h(x)}{\epsilon} && \quad \text{pois o limite de uma soma é a soma dos limites}\\
& = g'(x) + h'(x) && \quad \text{usando as definições de }g'(x) \text{ e } h'(x)
\end{align*}
$

Da mesma forma, é possível mostrar as seguintes regras importantes (incluí as provas no final deste notebook, caso você esteja curioso):

|                  | Função $f$ &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; | Derivada $f'$ &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; |
| ---------------- |------------------- | ------------------------------- |
| **Constante**     | $f(x) = c$         | $f'(x) = 0$                     |
| **Soma**          | $f(x) = g(x) + h(x)$ | $f'(x) = g'(x) + h'(x)$       |
| **Produto**      | $f(x) = g(x) h(x)$ | $f'(x) = g(x)h'(x) + g'(x)h(x)$ |
| **Quociente**     | $f(x) = \dfrac{g(x)}{h(x)}$ | $f'(x) = \dfrac{g'(x)h(x) - g(x)h'(x)}{h^2(x)}$ |
| **Potência**        | $f(x) = x^r$ com $r \neq 0$ | $f'(x) = rx^{r-1}$    |
| **Exponencial**  | $f(x) = \exp(x)$   | $f'(x)=\exp(x)$                 |
| **Logaritmo**    | $f(x) = \ln(x)$    | $f'(x) = \dfrac{1}{x} $         |
| **Seno**          | $f(x) = \sin(x)$   | $f'(x) = \cos(x) $              |
| **Cosseno**          | $f(x) = \cos(x)$   | $f'(x) = -\sin(x) $             |
| **Tangente**          | $f(x) = \tan(x)$   | $f'(x) = \dfrac{1}{\cos^2(x)}$  |
| **Regra da Cadeia**   | $f(x) = g(h(x))$ | $f'(x) = g'(h(x))\,h'(x)$  |

---

Vamos tentar diferenciar uma função simples usando as regras acima: encontraremos a derivada de $f(x)=x^3+\cos(x)$. Usando a regra da derivada da soma, descobrimos que $f'(x)=\dfrac{\mathrm{d}}{\mathrm{d}x}[x^3] + \dfrac{\mathrm{d}}{\mathrm{d}x}[\cos(x)]$. Usando a regra da derivada da potência e da função $\cos$, descobrimos que $f'(x) = 3x^2 - \sin(x)$.

---

Vamos tentar um exemplo mais difícil: vamos encontrar a derivada de $f(x) = \sin(2 x^2) + 1$. Primeiro, vamos definir $u(x)=\sin(x) + 1$ e $v(x) = 2x^2$. Usando a regra da soma, descobrimos que $u'(x)=\dfrac{\mathrm{d}}{\mathrm{d}x}[sin(x)] + \dfrac{\mathrm{d}}{\mathrm{d}x}[1]$. Como a derivada da função $\sin$ é $\cos$, e a derivada de constantes é 0, descobrimos que $u'(x)=\cos(x)$. Em seguida, usando a regra do produto, descobrimos que $v'(x)=2\dfrac{\mathrm{d}}{\mathrm{d}x}[x^2] + \dfrac{\mathrm{d}}{\mathrm{d}x}[2]\,x^2$. Como a derivada de uma constante é 0, o segundo termo se cancela. E como a regra da potência nos diz que a derivada de $x^2$ é $2x$, descobrimos que $v'(x)=4x$. Por fim, usando a regra da cadeia, como $f(x)=u(v(x))$, descobrimos que $f'(x)=u'(v(x))\,v'(x)=\cos(2x^2)\,4x$.

Vamos traçar $f$ seguido por $f'$, e vamos usar $f'(x_\mathbf{A})$ para encontrar a inclinação da tangente em algum ponto $\mathbf{A}$:

```run-python
#@title
animate_tangent(lambda x: np.sin(2*x**2) + 1, lambda x: 4*x*np.cos(2*x**2), r"\sin(2x^2)+1")
```

## A regra da cadeia

A regra da cadeia é mais fácil de lembrar usando a notação de Leibniz:

Se $f(x)=g(h(x))$ e $y=h(x)$, então: $\dfrac{\mathrm{d}f}{\mathrm{d}x} = \dfrac{\mathrm{d}f}{\mathrm{d}y} \dfrac{\mathrm{d}y}{\mathrm{d}x}$

De fato, $\dfrac{\mathrm{d}f}{\mathrm{d}y} = f'(y) = f'(h(x))$ e $\dfrac{\mathrm{d}y}{\mathrm{d}x}=h'(x)$.

É possível encadear muitas funções. Por exemplo, se $f(x)=g(h(i(x)))$, e definirmos $y=i(x)$ e $z=h(y)$, então $\dfrac{\mathrm{d}f}{\mathrm{d}x} = \dfrac{\mathrm{d}f}{\mathrm{d}z} \dfrac{\mathrm{d}z}{\mathrm{d}y} \dfrac{\mathrm{d}y}{\mathrm{d}x}$. Usando a notação de Lagrange, obtemos $f'(x)=g'(z)\,h'(y)\,i'(x)=g'(h(i(x)))\,h'(i(x))\,i'(x)$

A regra da cadeia é crucial no Aprendizado Profundo, pois uma rede neural é basicamente uma longa composição de funções. Por exemplo, uma rede neural densa de 3 camadas corresponde à seguinte função: $f(\mathbf{x})=\operatorname{Dense}_3(\operatorname{Dense}_2(\operatorname{Dense}_1(\mathbf{x})))$ (neste exemplo, $\operatorname{Dense}_3$ é a camada de saída).

# Derivadas e otimização

Ao tentar otimizar uma função $f(x)$, procuramos os valores de $x$ que minimizam (ou maximizam) a função.

É importante notar que, quando uma função atinge um mínimo ou máximo, assumindo que ela é diferenciável nesse ponto, a derivada será necessariamente igual a 0. Por exemplo, você pode verificar a animação acima e notar que sempre que a função $f$ (no gráfico superior) atinge um máximo ou mínimo, então a derivada $f'$ (no gráfico inferior) é igual a 0.

Portanto, uma maneira de otimizar uma função é diferenciá-la e encontrar analiticamente todos os valores para os quais a derivada é 0, então determinar quais desses valores otimizam a função (se houver). Por exemplo, considere a função $f(x)=\dfrac{1}{4}x^4 - x^2 + \dfrac{1}{2}$. Usando as regras de derivação (especificamente, a regra da soma, a regra do produto, a regra da potência e a regra da constante), descobrimos que $f'(x)=x^3 - 2x$. Procuramos os valores de $x$ para os quais $f'(x)=0$, então $x^3-2x=0$, e portanto $x(x^2-2)=0$. Então $x=0$, ou $x=\sqrt2$ ou $x=-\sqrt2$. Como você pode ver no gráfico seguinte de $f(x)$, esses 3 valores correspondem a extremos locais. Dois mínimos globais $f\left(\sqrt2\right)=f\left(-\sqrt2\right)=-\dfrac{1}{2}$ e um máximo local $f(0)=\dfrac{1}{2}$.

```python
#@title
def f(x):
  return 1/4 * x**4 - x**2 + 1/2

xs = np.linspace(-2.1, 2.1, 500)
ys = f(xs)
plt.plot(xs, ys)
plt.plot([np.sqrt(2), np.sqrt(2)], [0, f(np.sqrt(2))], "k--")
plt.plot([-np.sqrt(2), -np.sqrt(2)], [0, f(-np.sqrt(2))], "k--")
plt.text(-np.sqrt(2), 0.1, r"$-\sqrt{2}$",
         fontsize=14, horizontalalignment="center")
plt.text(np.sqrt(2), 0.1, r"$\sqrt{2}$",
         fontsize=14, horizontalalignment="center")
show(axis=[-2.1, 2.1, -1.4, 1.4], title=r"$y=f(x)=\dfrac{1}{4}x^4 - x^2 + 5$")
```

Se uma função tem um extremo local em um ponto $x_\mathrm{A}$ e é diferenciável nesse ponto, então $f'(x_\mathrm{A})=0$. No entanto, o inverso nem sempre é verdadeiro. Por exemplo, considere $f(x)=x^3$. Sua derivada é $f'(x)=x^2$, que é igual a 0 em $x_\mathrm{A}=0$. No entanto, esse ponto _não_ é um extremo, como você pode ver no diagrama a seguir. É apenas um único ponto onde a inclinação é 0.

```python
#@title
def f(x):
  return x**3

xs = np.linspace(-1.05, 1.05, 500)
ys = f(xs)
plt.plot(xs, ys)
show(axis=[-1.05, 1.05, -0.7, 0.7], title=r"$f(x)=x^3$")
```

Então, em resumo, você pode otimizar uma função trabalhando analiticamente os pontos em que a derivada é 0 e, em seguida, investigando apenas esses pontos. É uma solução lindamente elegante, mas requer muito trabalho e nem sempre é fácil, ou mesmo possível. Para redes neurais, é praticamente impossível.

Outra opção para otimizar uma função é realizar **Descida de Gradiente** (consideraremos minimizar a função, mas o processo seria quase idêntico se tentássemos maximizar uma função): comece em um ponto aleatório $x_0$, então use a derivada da função para determinar a inclinação nesse ponto e mova-se um pouco na direção descendente, então repita o processo até atingir um mínimo local e cruze os dedos na esperança de que isso aconteça de ser o mínimo global.

A cada iteração, o tamanho do passo é proporcional à inclinação, então o processo naturalmente desacelera à medida que se aproxima de um mínimo local. Cada passo também é proporcional à taxa de aprendizado: um parâmetro do próprio algoritmo de Descida de Gradiente (já que não é um parâmetro da função que estamos otimizando, é chamado de **hiperparâmetro**).

Aqui está uma animação desse processo para a função $f(x)=\dfrac{1}{4}x^4 - x^2 + \dfrac{1}{2}$:

```python
#@title
def animate_gradient_descent(f, fp, f_str, x_0):
    learning_rate = 0.01
    n_frames = 200
    x_min, x_max = -1000, 1000

    fig, (ax1, ax2) = plt.subplots(2, 1, figsize=(5, 8), sharex=True)

    # plot f
    xs = np.linspace(-2.1, 2.1, 500)
    ys = f(xs)
    ax1.plot(xs, ys)

    # plot tangent
    line_tangent, = ax1.plot([x_min, x_max], [0, 0])

    # plot f'
    xs = np.linspace(-2.1, 2.1, 500)
    ys = fp(xs)
    ax2.plot(xs, ys, "r-")

    # plot points A
    point_A1, = ax1.plot(0, 0, "bo")
    point_A2, = ax2.plot(0, 0, "bo")

    show([-2.1, 2.1, -1.4, 1.4], ax=ax1, ylabel="$f(x)$",
        title=r"$y=f(x)=" + f_str + "$ and the tangent at $x=x_\mathrm{A}$")
    show([-2.1, 2.1, -4.2, 4.2], ax=ax2, ylabel="$f'(x)$",
        title=r"$y=f'(x)$ and the slope of the tangent at $x=x_\mathrm{A}$")

    xs = []
    x = x_0
    for index in range(n_frames):
      xs.append(x)
      slope = fp(x)
      x = x - slope * learning_rate

    def update_graph(i):
        x = xs[i]
        f_x = f(x)
        df_dx = fp(x)
        offset = f_x - df_dx * x
        line_tangent.set_data([x_min, x_max],
                              [df_dx * x_min + offset, df_dx * x_max + offset])
        point_A1.set_data(x, f_x)
        point_A2.set_data(x, df_dx)
        return line_tangent, point_A1, point_A2

    anim = animation.FuncAnimation(fig, update_graph,
                                  init_func=lambda: update_graph(0),
                                  frames=n_frames,
                                  interval=20,
                                  blit=True)
    plt.close()
    return anim

def f(x):
  return 1/4 * x**4 - x**2 + 1/2

def fp(x):
  return x**3 - 2*x

animate_gradient_descent(f, fp, r"\dfrac{1}{4}x^4 - x^2 + \dfrac{1}{2}",
                         x_0=1/4)
```

Neste exemplo, começamos com $x_0 = \dfrac{1}{4}$, então a Descida de Gradiente "rolou para baixo" em direção ao valor mínimo em $x = \sqrt2$. Mas se tivéssemos começado em $x_0 = -\dfrac{1}{4}$, teria ido em direção a $-\sqrt2$. Isso ilustra o fato de que o valor inicial é importante: dependendo de $x_0$, o algoritmo pode convergir para um mínimo global (hurra!) ou para um mínimo local ruim (oh!) ou ficar preso em um platô, como um ponto de inflexão horizontal (oh!).

Existem muitas variantes do algoritmo de Descida de Gradiente, discutidas no Capítulo 11 do livro. Essas são as que nos importam no Aprendizado Profundo. Todas elas dependem da derivada da função de custo em relação aos parâmetros do modelo (discutiremos funções com vários parâmetros mais adiante neste notebook).

# Derivadas de ordem superior

O que acontece se tentarmos diferenciar a função $f'(x)$? Bem, obtemos a chamada derivada de segunda ordem, denotada por $f''(x)$, ou $\dfrac{\mathrm{d}^2f}{\mathrm{d}x^2}$. Se repetirmos o processo diferenciando $f''(x)$, obtemos a derivada de terceira ordem $f'''(x)$, ou $\dfrac{\mathrm{d}^3f}{\mathrm{d}x^3}$. E poderíamos continuar para obter derivadas de ordem superior.

Qual é a intuição por trás das derivadas de segunda ordem? Bem, como a derivada (de primeira ordem) representa a taxa instantânea de mudança de $f$ em cada ponto, a derivada de segunda ordem representa a taxa instantânea de mudança da própria taxa de mudança, em outras palavras, você pode pensar nisso como a **aceleração** da curva: se $f''(x) < 0$, então a curva está acelerando "para baixo", se $f''(x) > 0$ então a curva está acelerando "para cima", e se $f''(x) = 0$, então a curva é localmente uma linha reta. Observe que uma curva pode estar subindo (ou seja, $f'(x)>0$), mas também acelerando para baixo (ou seja, $f''(x) < 0$): por exemplo, imagine o caminho de uma pedra lançada para cima, enquanto é desacelerada pela gravidade (que constantemente acelera a pedra para baixo).

O Aprendizado Profundo geralmente usa apenas derivadas de primeira ordem, mas às vezes você encontrará alguns algoritmos de otimização ou funções de custo baseados em derivadas de segunda ordem.

# Derivadas parciais

Até agora, consideramos apenas funções com uma única variável $x$. O que acontece quando há várias variáveis? Por exemplo, vamos começar com uma função simples com 2 variáveis: $f(x,y)=\sin(xy)$. Se traçarmos essa função, usando $z=f(x,y)$, obtemos o seguinte gráfico 3D. Também tracei algum ponto $\mathrm{A}$ na superfície, junto com duas linhas que descreverei em breve.

```python
#@title
def plot_3d(f, title):
    fig = plt.figure(figsize=(8, 5))
    ax = fig.add_subplot(111, projection='3d')

    xs = np.linspace(-2.1, 2.1, 100)
    ys = np.linspace(-2.1, 2.1, 100)
    xs, ys = np.meshgrid(xs, ys)
    zs = f(xs, ys)

    surface = ax.plot_surface(xs, ys, zs,
                              cmap="coolwarm",
                              linewidth=0.3, edgecolor='k')

    ax.set_xlabel("$x$", fontsize=14)
    ax.set_ylabel("$y$", fontsize=14)
    ax.set_zlabel("$z$", fontsize=14)
    ax.set_title(title, fontsize=14)
    return ax

def plot_tangents(ax, x_A, y_A, f, df_dx, df_dy):
    ax.plot3D([x_A], [y_A], f(x_A, y_A), "bo", zorder=10)
    x_min, x_max = -2.1, 2.1
    slope_x = df_dx(x_A, y_A)
    offset_x = f(x_A, y_A) - slope_x * x_A
    ax.plot3D([x_min, x_max], [y_A, y_A],
              [slope_x * x_min + offset_x, slope_x * x_max + offset_x], "b-.",
              zorder=5)
    y_min, y_max = -2.1, 2.1
    slope_y = df_dy(x_A, y_A)
    offset_y = f(x_A, y_A) - slope_y * y_A
    ax.plot3D([x_A, x_A], [y_min, y_max],
              [slope_y * y_min + offset_y, slope_y * y_max + offset_y], "r-",
              zorder=5)

def f(x, y):
    return np.sin(x * y)

def df_dx(x, y):
    return y * np.cos(x * y)

def df_dy(x, y):
    return x * np.cos(x * y)

ax = plot_3d(f, r"$z = f(x, y) = \sin(xy)$")
plot_tangents(ax, 0.1, -1, f, df_dx, df_dy)

plt.show()
```

Se você estivesse em pé nessa superfície no ponto $\mathrm{A}$ e caminhasse ao longo do eixo $x$ para a direita (aumentando $x$), seu caminho desceria bastante (ao longo da linha tracejada azul). A inclinação ao longo desse eixo seria negativa. No entanto, se você caminhasse ao longo do eixo $y$, para trás (aumentando $y$), então seu caminho seria quase plano (ao longo da linha sólida vermelha), pelo menos localmente: a inclinação ao longo desse eixo, no ponto $\mathrm{A}$, seria ligeiramente positiva.

Como você pode ver, um único número não é mais suficiente para descrever a inclinação da função em um determinado ponto. Precisamos de uma inclinação para o eixo $x$ e uma inclinação para o eixo $y$. Uma inclinação para cada variável. Para encontrar a inclinação ao longo do eixo $x$, chamada de **derivada parcial de $f$ em relação a $x$**, e denotada por $\dfrac{\partial f}{\partial x}$ (com $\partial$ curvado), podemos diferenciar $f(x,y)$ em relação a $x$ enquanto tratamos todas as outras variáveis (neste caso, apenas $y$) como constantes:

$ \dfrac{\partial f}{\partial x} = \underset{\epsilon \to 0}\lim\dfrac{f(x+\epsilon, y) - f(x,y)}{\epsilon}$

Se você usar as regras de derivação listadas anteriormente (neste exemplo, você precisaria apenas da regra do produto e da regra da cadeia), tomando cuidado para tratar $y$ como uma constante, você encontrará:

$ \dfrac{\partial f}{\partial x} = y\cos(xy)$

Da mesma forma, a derivada parcial de $f$ em relação a $y$ é definida como:

$ \dfrac{\partial f}{\partial y} = \underset{\epsilon \to 0}\lim\dfrac{f(x, y+\epsilon) - f(x,y)}{\epsilon}$

Todas as variáveis, exceto $y$, são tratadas como constantes (apenas $x$ neste exemplo). Usando as regras de derivação, obtemos:

$ \dfrac{\partial f}{\partial y} = x\cos(xy)$

Agora temos equações para calcular a inclinação ao longo do eixo $x$ e ao longo do eixo $y$. Mas e as outras direções? Se você estivesse em pé na superfície no ponto $\mathrm{A}$, poderia decidir caminhar em qualquer direção que escolhesse, não apenas ao longo dos eixos $x$ ou $y$. Qual seria a inclinação então? Não deveríamos calcular a inclinação em todas as direções possíveis?

Bem, pode-se mostrar que, se todas as derivadas parciais são definidas e contínuas em uma vizinhança ao redor do ponto $\mathrm{A}$, então a função $f$ é **totalmente diferenciável** nesse ponto, o que significa que pode ser localmente aproximada por um plano $P_\mathrm{A}$ (o plano tangente à superfície no ponto $\mathrm{A}$). Nesse caso, ter apenas as derivadas parciais ao longo de cada eixo ($x$ e $y$ no nosso caso) é suficiente para caracterizar perfeitamente esse plano. Sua equação é:

$z = f(x_\mathrm{A},y_\mathrm{A}) + (x - x_\mathrm{A})\dfrac{\partial f}{\partial x}(x_\mathrm{A},y_\mathrm{A}) + (y - y_\mathrm{A})\dfrac{\partial f}{\partial y}(x_\mathrm{A},y_\mathrm{A})$

No Aprendizado Profundo, geralmente lidaremos com funções bem comportadas que são totalmente diferenciáveis em qualquer ponto onde todas as derivadas parciais são definidas, mas você deve saber que algumas funções não são tão legais. Por exemplo, considere a função:

$h(x,y)=\begin{cases}0 \text { se } x=0 \text{ ou } y=0\\1 \text { caso contrário}\end{cases}$

Na origem (ou seja, em $(x,y)=(0,0)$), as derivadas parciais da função $h$ em relação a $x$ e $y$ são ambas perfeitamente definidas: são iguais a 0. No entanto, a função claramente não pode ser aproximada por um plano nesse ponto. Ela não é totalmente diferenciável nesse ponto (mas é totalmente diferenciável em qualquer ponto fora dos eixos).

# Gradientes

Até agora, consideramos apenas funções com uma única variável $x$, ou com 2 variáveis, $x$ e $y$, mas o parágrafo anterior também se aplica a funções com mais variáveis. Então, vamos considerar uma função $f$ com $n$ variáveis: $f(x_1, x_2, \dots, x_n)$. Por conveniência, vamos definir um vetor $\mathbf{x}$ cujos componentes são essas variáveis:

$\mathbf{x}=\begin{pmatrix}
x_1\\
x_2\\
\vdots\\
x_n
\end{pmatrix}$

Agora $f(\mathbf{x})$ é mais fácil de escrever do que $f(x_1, x_2, \dots, x_n)$.

O gradiente da função $f(\mathbf{x})$ em algum ponto $\mathbf{x}_\mathrm{A}$ é o vetor cujos componentes são todas as derivadas parciais da função nesse ponto. Ele é denotado por $\nabla f(\mathbf{x}_\mathrm{A})$, ou às vezes $\nabla_{\mathbf{x}_\mathrm{A}}f$:

$\nabla f(\mathbf{x}_\mathrm{A}) = \begin{pmatrix}
\dfrac{\partial f}{\partial x_1}(\mathbf{x}_\mathrm{A})\\
\dfrac{\partial f}{\partial x_2}(\mathbf{x}_\mathrm{A})\\
\vdots\\
\dfrac{\partial f}{\partial x_n}(\mathbf{x}_\mathrm{A})\\
\end{pmatrix}$

Assumindo que a função é totalmente diferenciável no ponto $\mathbf{x}_\mathbf{A}$, então a superfície que ela descreve pode ser aproximada por um plano nesse ponto (como discutido na seção anterior), e o vetor gradiente é aquele que aponta para a inclinação mais íngreme nesse plano.

## Descida de Gradiente, revisitada

No Aprendizado Profundo, o algoritmo de Descida de Gradiente que discutimos anteriormente é baseado em gradientes em vez de derivadas (daí o seu nome). Ele funciona de maneira muito semelhante, mas usando vetores em vez de escalares: simplesmente comece com um vetor aleatório $\mathbf{x}_0$, então calcule o gradiente de $f$ nesse ponto e dê um pequeno passo na direção oposta, então repita até a convergência. Mais precisamente, a cada passo $t$, calcule $\mathbf{x}_t = \mathbf{x}_{t-1} - \eta \nabla f(\mathbf{x}_{t-1})$. A constante $\eta$ é a taxa de aprendizado, tipicamente um valor pequeno como $10^{-3}$. Na prática, geralmente usamos variantes mais eficientes desse algoritmo, mas a ideia geral permanece a mesma.

No Aprendizado Profundo, a letra $\mathbf{x}$ é geralmente usada para representar os dados de entrada. Quando você _usa_ uma rede neural para fazer previsões, você alimenta a rede neural com as entradas $\mathbf{x}$, e você obtém uma previsão $\hat{y} = f(\mathbf{x})$. A função $f$ trata os parâmetros do modelo como constantes. Podemos usar uma notação mais explícita escrevendo $\hat{y} = f_\mathbf{w}(\mathbf{x})$, onde $\mathbf{w}$ representa os parâmetros do modelo e indica que a função depende deles, mas os trata como constantes.

No entanto, ao _treinar_ uma rede neural, fazemos exatamente o oposto: todos os exemplos de treinamento são agrupados em uma matriz $\mathbf{X}$, todos os rótulos são agrupados em um vetor $\mathbf{y}$, e ambos $\mathbf{X}$ e $\mathbf{y}$ são tratados como constantes, enquanto $\mathbf{w}$ é tratado como variável: especificamente, tentamos minimizar a função de custo $\mathcal L_{\mathbf{X}, \mathbf{y}}(\mathbf{w}) = g(f_{\mathbf{X}}(\mathbf{w}), \mathbf{y})$, onde $g$ é uma função que mede a "discrepância" entre as previsões $f_{\mathbf{X}}(\mathbf{w})$ e os rótulos $\mathbf{y}$, onde $f_{\mathbf{X}}(\mathbf{w})$ representa o vetor contendo as previsões para cada exemplo de treinamento. A minimização da função de custo é geralmente realizada usando Descida de Gradiente (ou uma variante da DG): começamos com parâmetros aleatórios do modelo $\mathbf{w}_0$, então calculamos $\nabla \mathcal L(\mathbf{w}_0)$ e usamos esse vetor gradiente para realizar um passo de Descida de Gradiente, então repetimos o processo até a convergência. É crucial entender que o gradiente da função de custo é em relação aos parâmetros do modelo $\mathbf{w}$ (_não_ as entradas $\mathbf{x}$).

# Jacobianos

Até agora, consideramos apenas funções que produzem um escalar, mas é possível produzir vetores. Por exemplo, uma rede neural de classificação geralmente produz uma probabilidade para cada classe, então, se houver $m$ classes, a rede neural produzirá um vetor $d$-dimensional para cada entrada.

No Aprendizado Profundo, geralmente só precisamos diferenciar a função de custo, que quase sempre produz um único número escalar. Mas suponha por um segundo que você queira diferenciar uma função $\mathbf{f}(\mathbf{x})$ que produz vetores $d$-dimensionais. A boa notícia é que você pode tratar cada dimensão de _saída_ independentemente das outras. Isso lhe dará uma derivada parcial para cada dimensão de entrada e cada dimensão de saída. Se você colocá-las todas em uma única matriz, com uma coluna por dimensão de entrada e uma linha por dimensão de saída, você obterá a chamada **matriz jacobiana**.

$
\mathbf{J}_\mathbf{f}(\mathbf{x}_\mathbf{A}) = \begin{pmatrix}
\dfrac{\partial f_1}{\partial x_1}(\mathbf{x}_\mathbf{A})
&& \dfrac{\partial f_1}{\partial x_2}(\mathbf{x}_\mathbf{A})
&& \dots
&& \dfrac{\partial f_1}{\partial x_n}(\mathbf{x}_\mathbf{A})\\
\dfrac{\partial f_2}{\partial x_1}(\mathbf{x}_\mathbf{A})
&& \dfrac{\partial f_2}{\partial x_2}(\mathbf{x}_\mathbf{A})
&& \dots
&& \dfrac{\partial f_2}{\partial x_n}(\mathbf{x}_\mathbf{A})\\
\vdots && \vdots && \ddots && \vdots \\
\dfrac{\partial f_m}{\partial x_1}(\mathbf{x}_\mathbf{A})
&& \dfrac{\partial f_m}{\partial x_2}(\mathbf{x}_\mathbf{A})
&& \dots
&& \dfrac{\partial f_m}{\partial x_n}(\mathbf{x}_\mathbf{A})
\end{pmatrix}
$

As próprias derivadas parciais são frequentemente chamadas de **jacobianos**. São apenas as derivadas parciais de primeira ordem da função $\mathbf{f}$.

# Hessianos

Vamos voltar a uma função $f(\mathbf{x})$ que recebe um vetor $n$-dimensional como entrada e produz um escalar. Se você determinar a equação da derivada parcial de $f$ em relação a $x_i$ (o $i^\text{ésimo}$ componente de $\mathbf{x}$), você obterá uma nova função de $\mathbf{x}$: $\dfrac{\partial f}{\partial x_i}$. Você pode então calcular a derivada parcial dessa função em relação a $x_j$ (o $j^\text{ésimo}$ componente de $\mathbf{x}$). O resultado é uma derivada parcial de uma derivada parcial: em outras palavras, é uma **derivada parcial de segunda ordem**, também chamada de **hessiana**. Ela é denotada por $\mathbf{x}$: $\dfrac{\partial^2 f}{\partial x_jx_i}$. Se $i\neq j$ então é chamada de **derivada parcial mista de segunda ordem**.
Ou, se $j=i$, é denotada por $\dfrac{\partial^2 f}{\partial {x_i}^2}$

Vamos ver um exemplo: $f(x, y)=\sin(xy)$. Como mostramos anteriormente, as derivadas parciais de primeira ordem de $f$ são: $\dfrac{\partial f}{\partial x}=y\cos(xy)$ e $\dfrac{\partial f}{\partial y}=x\cos(xy)$. Então, podemos agora calcular todas as hessianas (usando as regras de derivação que discutimos anteriormente):

* $\dfrac{\partial^2 f}{\partial x^2} = \dfrac{\partial f}{\partial x}\left[y\cos(xy)\right] = -y^2\sin(xy)$
* $\dfrac{\partial^2 f}{\partial y\,\partial x} = \dfrac{\partial f}{\partial y}\left[y\cos(xy)\right] = \cos(xy) - xy\sin(xy)$
* $\dfrac{\partial^2 f}{\partial x\,\partial y} = \dfrac{\partial f}{\partial x}\left[x\cos(xy)\right] = \cos(xy) - xy\sin(xy)$
* $\dfrac{\partial^2 f}{\partial y^2} = \dfrac{\partial f}{\partial y}\left[x\cos(xy)\right] = -x^2\sin(xy)$

Observe que $\dfrac{\partial^2 f}{\partial x\,\partial y} = \dfrac{\partial^2 f}{\partial y\,\partial x}$. Esse é o caso sempre que todas as derivadas parciais são definidas e contínuas em uma vizinhança ao redor do ponto em que diferenciamos.

A matriz contendo todas as hessianas é chamada de **matriz hessiana**:

$
\mathbf{H}_f(\mathbf{x}_\mathbf{A}) = \begin{pmatrix}
\dfrac{\partial^2 f}{\partial {x_1}^2}(\mathbf{x}_\mathbf{A})
&& \dfrac{\partial^2 f}{\partial x_1\, \partial x_2}(\mathbf{x}_\mathbf{A})
&& \dots
&& \dfrac{\partial^2 f}{\partial x_1\, \partial x_n}(\mathbf{x}_\mathbf{A})\\
\dfrac{\partial^2 f}{\partial x_2\,\partial x_1}(\mathbf{x}_\mathbf{A})
&& \dfrac{\partial^2 f}{\partial {x_2}^2}(\mathbf{x}_\mathbf{A})
&& \dots
&& \dfrac{\partial^2 f}{\partial x_2\, \partial x_n}(\mathbf{x}_\mathbf{A})\\
\vdots && \vdots && \ddots && \vdots \\
\dfrac{\partial^2 f}{\partial x_n\,\partial x_1}(\mathbf{x}_\mathbf{A})
&& \dfrac{\partial^2 f}{\partial x_n\,\partial x_2}(\mathbf{x}_\mathbf{A})
&& \dots
&& \dfrac{\partial^2 f}{\partial {x_n}^2}(\mathbf{x}_\mathbf{A})\\
\end{pmatrix}
$

Existem ótimos algoritmos de otimização que tiram proveito das hessianas, mas, na prática, o Aprendizado Profundo quase nunca as usa. De fato, se uma função tem $n$ variáveis, há $n^2$ hessianas: como as redes neurais geralmente têm vários milhões de parâmetros, o número de hessianas excederia milhares de bilhões. Mesmo se tivéssemos a quantidade necessária de RAM, os cálculos seriam proibitivamente lentos.

## Algumas provas

Vamos terminar provando todas as regras de derivação que listamos anteriormente. Você não precisa passar por todas essas provas para ser um bom praticante de Aprendizado Profundo, mas isso pode ajudá-lo a obter uma compreensão mais profunda das derivadas.

## Constante: $f(x)=c$

$
\begin{align*}
f'(x) & = \underset{\epsilon \to 0}\lim\dfrac{f(x+\epsilon) - f(x)}{\epsilon} && \quad\text{por definição}\\
& = \underset{\epsilon \to 0}\lim\dfrac{c - c}{\epsilon} && \quad \text{usando }f(x) = c \\
& = \underset{\epsilon \to 0}\lim 0 && \quad \text{pois }c - c = 0\\
& = 0 && \quad \text{pois o limite de uma constante é essa constante}
\end{align*}
$

## Regra do produto: $f(x)=g(x)h(x)$

$
\begin{align*}
f'(x) & = \underset{\epsilon \to 0}\lim\dfrac{f(x+\epsilon) - f(x)}{\epsilon} && \quad\text{por definição}\\
& = \underset{\epsilon \to 0}\lim\dfrac{g(x+\epsilon)h(x+\epsilon) - g(x)h(x)}{\epsilon} && \quad \text{usando }f(x) = g(x)h(x) \\
& = \underset{\epsilon \to 0}\lim\dfrac{g(x+\epsilon)h(x+\epsilon) - g(x)h(x+\epsilon) + g(x)h(x + \epsilon) - g(x)h(x)}{\epsilon} && \quad \text{subtraindo e adicionando }g(x)h(x + \epsilon)\\
& = \underset{\epsilon \to 0}\lim\dfrac{g(x+\epsilon)h(x+\epsilon) - g(x)h(x+\epsilon)}{\epsilon} + \underset{\epsilon \to 0}\lim\dfrac{g(x)h(x + \epsilon) - g(x)h(x)}{\epsilon} && \quad \text{pois o limite de uma soma é a soma dos limites}\\
& = \underset{\epsilon \to 0}\lim{\left[\dfrac{g(x+\epsilon) - g(x)}{\epsilon}h(x+\epsilon)\right]} \,+\, \underset{\epsilon \to 0}\lim{\left[g(x)\dfrac{h(x + \epsilon) - h(x)}{\epsilon}\right]} && \quad \text{fatorando }h(x+\epsilon) \text{ e } g(x)\\
& = \underset{\epsilon \to 0}\lim{\left[\dfrac{g(x+\epsilon) - g(x)}{\epsilon}h(x+\epsilon)\right]} \,+\, g(x)\underset{\epsilon \to 0}\lim{\dfrac{h(x + \epsilon) - h(x)}{\epsilon}} && \quad \text{tirando } g(x) \text{ do limite pois não depende de }\epsilon\\
& = \underset{\epsilon \to 0}\lim{\left[\dfrac{g(x+\epsilon) - g(x)}{\epsilon}h(x+\epsilon)\right]} \,+\, g(x)h'(x) && \quad \text{usando a definição de }h'(x)\\
& = \underset{\epsilon \to 0}\lim{\left[\dfrac{g(x+\epsilon) - g(x)}{\epsilon}\right]}\underset{\epsilon \to 0}\lim{h(x+\epsilon)} + g(x)h'(x) && \quad \text{pois o limite de um produto é o produto dos limites}\\
& = \underset{\epsilon \to 0}\lim{\left[\dfrac{g(x+\epsilon) - g(x)}{\epsilon}\right]}h(x) + h(x)g'(x) && \quad \text{pois } h(x) \text{ é contínua}\\
& = g'(x)h(x) + g(x)h'(x) && \quad \text{usando a definição de }g'(x)
\end{align*}
$

Observe que se $g(x)=c$ (uma constante), então $g'(x)=0$, então a equação se simplifica para:

$f'(x)=c \, h'(x)$

## Regra da cadeia: $f(x)=g(h(x))$

$
\begin{align*}
f'(x) & = \underset{\epsilon \to 0}\lim\dfrac{f(x+\epsilon) - f(x)}{\epsilon} && \quad\text{por definição}\\
& = \underset{\epsilon \to 0}\lim\dfrac{g(h(x+\epsilon)) - g(h(x))}{\epsilon} && \quad \text{usando }f(x) = g(h(x))\\
& = \underset{\epsilon \to 0}\lim{\left[\dfrac{h(x+\epsilon)-h(x)}{h(x+\epsilon)-h(x)}\,\dfrac{g(h(x+\epsilon)) - g(h(x))}{\epsilon}\right]} && \quad \text{multiplicando e dividindo por }h(x+\epsilon) - h(x)\\
& = \underset{\epsilon \to 0}\lim{\left[\dfrac{h(x+\epsilon)-h(x)}{\epsilon}\,\dfrac{g(h(x+\epsilon)) - g(h(x))}{h(x+\epsilon)-h(x)}\right]} && \quad \text{trocando os denominadores}\\
& = \underset{\epsilon \to 0}\lim{\left[\dfrac{h(x+\epsilon)-h(x)}{\epsilon}\right]} \underset{\epsilon \to 0}\lim{\left[\dfrac{g(h(x+\epsilon)) - g(h(x))}{h(x+\epsilon)-h(x)}\right]} && \quad \text{o limite de um produto é o produto dos limites}\\
& = h'(x) \underset{\epsilon \to 0}\lim{\left[\dfrac{g(h(x+\epsilon)) - g(h(x))}{h(x+\epsilon)-h(x)}\right]} && \quad \text{usando a definição de }h'(x)\\
& = h'(x) \underset{\epsilon \to 0}\lim{\left[\dfrac{g(u) - g(v)}{u-v}\right]} && \quad \text{usando }u=h(x+\epsilon) \text{ e } v=h(x)\\
& = h'(x) \underset{u \to v}\lim{\left[\dfrac{g(u) - g(v)}{u-v}\right]} && \quad \text{ pois } h \text{ é contínua, então } \underset{\epsilon \to 0}\lim{u}=v\\
& = h'(x)g'(v) && \quad \text{ usando a definição de } g'(v)\\
& = h'(x)g'(h(x)) && \quad \text{ pois } v = h(x)
\end{align*}
$

## Exponencial: $f(x)=\exp(x)=e^x$

Existem várias definições equivalentes do número $e$. Uma delas afirma que $e$ é o único número positivo para o qual $\underset{\epsilon \to 0}\lim{\dfrac{e^\epsilon - 1}{\epsilon}}=1$. Vamos usar isso nesta prova:

$
\begin{align*}
f'(x) & = \underset{\epsilon \to 0}\lim\dfrac{f(x+\epsilon) - f(x)}{\epsilon} && \quad\text{por definição}\\
& = \underset{\epsilon \to 0}\lim\dfrac{e^{x+\epsilon} - e^x}{\epsilon} && \quad \text{usando }f(x) = e^x\\
& = \underset{\epsilon \to 0}\lim\dfrac{e^x e^\epsilon - e^x}{\epsilon} && \quad \text{usando o fato de que } x^{a+b}=x^a x^b\\
& = \underset{\epsilon \to 0}\lim{\left[e^x\dfrac{e^\epsilon - 1}{\epsilon}\right]} && \quad \text{fatorando }e^x\\
& = \underset{\epsilon \to 0}\lim{e^x} \, \underset{\epsilon \to 0}\lim{\dfrac{e^\epsilon - 1}{\epsilon}} && \quad \text{o limite de um produto é o produto dos limites}\\
& = \underset{\epsilon \to 0}\lim{e^x} && \quad \text{pois }\underset{\epsilon \to 0}\lim{\dfrac{e^\epsilon - 1}{\epsilon}}=1\\
& = e^x && \quad \text{pois } e^x \text{ não depende de }\epsilon
\end{align*}
$

## Logaritmo: $f(x) = \ln(x)$

Outra definição do número $e$ é:

$e = \underset{n \to \infty}\lim\left(1+\dfrac{1}{n}\right)^n$

Definindo $\epsilon = \dfrac{1}{n}$, podemos reescrever a definição anterior como:

$e = \underset{\epsilon \to 0}\lim\left(1+\epsilon\right)^{1/\epsilon}$

Isso será útil em um segundo:

$
\begin{align*}
f'(x) & = \underset{\epsilon \to 0}\lim\dfrac{f(x+\epsilon) - f(x)}{\epsilon} && \quad\text{por definição}\\
& = \underset{\epsilon \to 0}\lim\dfrac{\ln(x+\epsilon) - \ln(x)}{\epsilon} && \quad \text{usando }f(x) = \ln(x)\\
& = \underset{\epsilon \to 0}\lim\dfrac{\ln\left(\dfrac{x+\epsilon}{x}\right)}{\epsilon} && \quad \text{pois }\ln(a)-\ln(b)=\ln\left(\dfrac{a}{b}\right)\\
& = \underset{\epsilon \to 0}\lim{\left[\dfrac{1}{\epsilon} \, \ln\left(1 + \dfrac{\epsilon}{x}\right)\right]} && \quad \text{apenas reorganizando um pouco}\\
& = \underset{\epsilon \to 0}\lim{\left[\dfrac{1}{xu} \, \ln\left(1 + u\right)\right]} && \quad \text{definindo }u=\dfrac{\epsilon}{x} \text{ e assim } \epsilon=xu\\
& = \underset{u \to 0}\lim{\left[\dfrac{1}{xu} \, \ln\left(1 + u\right)\right]} && \quad \text{substituindo } \underset{\epsilon \to 0}\lim \text{ por } \underset{u \to 0}\lim \text{ pois }\underset{\epsilon \to 0}\lim u=0\\
& = \underset{u \to 0}\lim{\left[\dfrac{1}{x} \, \ln\left((1 + u)^{1/u}\right)\right]} && \quad \text{pois }a\ln(b)=\ln(b^a)\\
& = \dfrac{1}{x}\underset{u \to 0}\lim{\left[\ln\left((1 + u)^{1/u}\right)\right]} && \quad \text{tirando }\dfrac{1}{x} \text{ do limite pois não depende de }\epsilon\\
& = \dfrac{1}{x}\ln\left(\underset{u \to 0}\lim{(1 + u)^{1/u}}\right) && \quad \text{tirando }\ln\text{ do limite pois é uma função contínua}\\
& = \dfrac{1}{x}\ln(e) && \quad \text{pois }e=\underset{u \to 0}\lim{(1 + u)^{1/u}}\\
& = \dfrac{1}{x} && \quad \text{pois }\ln(e)=1
\end{align*}
$

## Regra da potência: $f(x)=x^r$, com $r \neq 0$

Vamos definir $g(x)=e^x$ e $h(x)=\ln(x^r)$. Como $a = e^{\ln(a)}$, podemos reescrever $f$ como $f(x)=g(h(x))$, o que nos permite usar a regra da cadeia:

$f'(x) = h'(x)g'(h(x))$

Sabemos a derivada da exponencial: $g'(x)=e^x$. Também sabemos a derivada do logaritmo natural: $\ln'(x)=\dfrac{1}{x}$ então $h'(x)=\dfrac{r}{x}$. Portanto:

$f'(x) = \dfrac{r}{x} e^{\ln(x^r)}$

Como $e^{\ln(a)} = a$, esta equação se simplifica para:

$f'(x) = \dfrac{r}{x} x^r$

E finalmente:

$f'(x) = rx^{r - 1}$

Observe que a regra da potência funciona para qualquer $r \neq 0$, incluindo números negativos e números reais. Por exemplo:

* se $f(x) = \dfrac{1}{x} = x^{-1}$, então $f'(x)=-x^{-2}=-\dfrac{1}{x^2}$.
* se $f(x) = \sqrt{x} = x^{1/2}$, então $f'(x)=\dfrac{1}{2}x^{-1/2}=\dfrac{1}{2\sqrt{x}}$

## Inverso multiplicativo: $f(x)=\dfrac{1}{h(x)}$
Primeiro, vamos definir $g(x) = \dfrac{1}{x}$. Isso leva a $f(x)=g(h(x))$.
Agora podemos usar a regra da cadeia:

$f'(x) = h'(x)g'(h(x))$

Como $g(x)=x^{-1}$, podemos usar a regra da potência para encontrar $g'(x)=-\dfrac{1}{x^2}$

Finalmente, obtemos:

$f'(x) = -\dfrac{h'(x)}{h^2(x)}$

## Regra do quociente: $f(x)=\dfrac{g(x)}{h(x)}$

Vamos reescrever $f(x)$ como um produto: $f(x)=g(x)u(x)$ com $u(x)=\dfrac{1}{h(x)}$

Agora podemos usar a regra do produto para obter:

$f(x) = g'(x)u(x) + g(x)u'(x)$

Substituindo $u(x)$ por $\dfrac{1}{h(x)}$ e usando o resultado da seção anterior para substituir $u'(x)$ por $\dfrac{-h'(x)}{h^2(x)}$, obtemos:

$f(x) = g'(x)\dfrac{1}{h(x)} + g(x)\dfrac{-h'(x)}{h^2(x)}$

Agora multiplicamos e dividimos o primeiro termo por $h(x)$:

$f(x) = \dfrac{g'(x)h(x)}{h^2(x)} - \dfrac{g(x)h'(x)}{h^2(x)}$

E finalmente:

$f(x) = \dfrac{g'(x)h(x) - g(x)h'(x)}{h^2(x)}$

## Seno: $f(x)=\sin(x)$

Para esta prova, primeiro precisaremos provar que $\underset{\theta \to 0}\lim\dfrac{\sin(\theta)}{\theta}=1$. Uma maneira de fazer isso é considerar o seguinte diagrama:

```python
#@title
angle = np.pi/5
A_pos = [np.cos(angle), np.sin(angle)]

fig, ax = plt.subplots(figsize=(6, 6))

from functools import partial
ax_text = partial(ax.text, color="w", fontsize=18, zorder=4,
                  horizontalalignment='center', verticalalignment='center')

circle = plt.Circle((0, 0), 1,
                    zorder=0, facecolor='w', edgecolor='k', linestyle="--")
triangle1 = plt.Polygon([[0, 0], [1, np.tan(angle)], [1, 0]],
                        zorder=1, facecolor='r', edgecolor='k')
arc_points = np.array([[0, 0]] + [[np.cos(a), np.sin(a)]
              for a in np.linspace(0, angle, 50)])
ax.fill(arc_points[:, 0], arc_points[:, 1],
        zorder=2, facecolor='c', edgecolor='k')
triangle2 = plt.Polygon([[0, 0], A_pos, [A_pos[0], 0]],
                        zorder=3, facecolor='b', edgecolor='k')
ax_text(2*np.cos(angle)/3, np.sin(angle)/4, "A")
ax_text((1+np.cos(angle))/2, np.sin(angle)/4, "B")
ax_text((1+np.cos(angle))/2, 0.9*np.sin(angle), "C")
ax_text(0.25*np.cos(angle/2), 0.25*np.sin(angle/2), r"$\theta$")
arc = mpl.patches.Arc([0, 0], 2*0.2, 2*0.2, theta1=0, theta2=angle*180/np.pi,
                      zorder=5, color='y', linewidth=3)
ax_text(0.03, -0.05, "0", color='k')
ax_text(1.03, -0.05, "1", color='k')

ax.axhline(y=0, color='k', zorder=4)
ax.axvline(x=0, color='k', zorder=4)
ax.axvline(x=1, color='k', zorder=4, linewidth=1, linestyle='--')
ax.axis('equal')
ax.axis([-0.1, 1.1, -0.1, 1.1])
ax.axis('off')
ax.add_artist(circle)
ax.add_artist(triangle1)
ax.add_artist(triangle2)
ax.add_patch(arc)
plt.show()
```

O círculo é o círculo unitário (raio=1).

Assumindo $0 < \theta < \dfrac{\pi}{2}$, a área do triângulo azul (área $\mathrm{A}$) é igual à sua altura ($\sin(\theta)$) vezes sua base ($\cos(\theta)$) dividida por 2. Então $\mathrm{A} = \dfrac{1}{2}\sin(\theta)\cos(\theta)$.

O círculo unitário tem uma área de $\pi$, então o setor circular (em forma de fatia de pizza) tem uma área de A + B = $\pi\dfrac{\theta}{2\pi} = \dfrac{\theta}{2}$.

Em seguida, o triângulo grande (A + B + C) tem uma área igual à sua altura ($\tan(\theta)$) multiplicada por sua base (de comprimento 1) dividida por 2, então A + B + C = $\dfrac{\tan(\theta)}{2}$.

Quando $0 < \theta < \dfrac{\pi}{2}$, temos $\mathrm{A} < \mathrm{A} + \mathrm{B} < \mathrm{A} + \mathrm{B} + \mathrm{C}$, portanto:

$\dfrac{1}{2}\sin(\theta)\cos(\theta) < \dfrac{\theta}{2} < \dfrac{\tan(\theta)}{2}$

Podemos multiplicar todos os termos por 2 para nos livrarmos dos fatores $\dfrac{1}{2}$. Também podemos dividir por $\sin(\theta)$, que é estritamente positivo (assumindo $0 < \theta < \dfrac{\pi}{2}$), então as desigualdades ainda se mantêm:

$cos(\theta) < \dfrac{\theta}{\sin(\theta)} < \dfrac{\tan(\theta)}{\sin(\theta)}$

Lembre-se de que $\tan(\theta)=\dfrac{\sin(\theta)}{\cos(\theta)}$, então o último termo se simplifica assim:

$cos(\theta) < \dfrac{\theta}{\sin(\theta)} < \dfrac{1}{\cos(\theta)}$

Como todos esses termos são estritamente positivos quando $0 < \theta <