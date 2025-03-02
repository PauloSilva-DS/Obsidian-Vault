---
title: "5 Cool Jupyter Notebook Dicas | by Ben Hui | em Towards Dev - Freedium"
source: "https://freedium.cfd/https://medium.com/towardsdev/5-cool-jupyter-notebook-tips-0d2c00b5ffb8"
author:
published:
created: 2025-03-01
description: "Jupyter Notebook is one of the most popular integrated development environments (IDEs) for almost..."
tags:
  - "clippings"
---
O Jupyter Notebook é um dos ambientes de desenvolvimento integrados (IDEs) mais populares para quase todas as tarefas de programação Python, como ciência de dados, aprendizado de máquina, computação científica e muito mais.

Seus recursos de codificação interativo o tornam uma ferramenta preferida não apenas para iniciantes, mas também para especialistas.

No entanto, apesar de seu uso generalizado, muitos usuários não aproveitam totalmente seu potencial.

Como resultado, eles geralmente aderem à interface e funcionalidade padrão do Jupyter, que, na minha opinião, pode ser significativamente aprimorada para fornecer uma experiência mais rica.

Neste artigo, vou apresentar 5 dicas legais de Jupyter que você pode nem saber que existem.

Essas dicas irão capacitá-lo a alcançar novos níveis de produtividade e criatividade com esta ferramenta poderosa.

Vamos começar!

1. **Pare de pré-visualizar dados brutos**

Normalmente, quando carregamos um DataFrame no Jupyter, nós o visualizamos imprimindo-o. Por exemplo:

```python
df
```

![Nenhum](https://miro.medium.com/v2/resize:fit:700/0*Q9UChKLGBuBBvYwK)

No entanto, isso quase não fornece informações sobre o conteúdo dos dados.

Como resultado, os usuários geralmente precisam analisá-lo para obter insights mais profundos, o que envolve escrever código simples, mas repetitivo.

Em vez disso, use Jupyter-DataTables. Você pode instalá-lo usando as seguintes etapas:

```python
pip install jupyter-datatables
```

Para usá-lo, execute o seguinte código no Jupyter:

```python
from jupyter_datatables import init_datatables_mode

init_datatables_mode()
```

Ele melhora a visualização padrão do DataFrame com muitos recursos úteis.

Portanto, sempre que você imprimir um DataFrame, ele será exibido de forma mais elegante, como mostrado abaixo.

![Nenhum](https://miro.medium.com/v2/resize:fit:700/0*73ZZWKYnrfs-fme0)

Esta visualização mais rica fornece operações de classificação, filtragem, exportação e paginação, bem como distribuição de colunas e tipos de dados.

**2\. Rotular dados clicando em um botão** Nem todos os dados são pré-rotulados quando adquiridos. Portanto, ao trabalhar com dados não rotulados, muitas vezes você pode precisar passar algum tempo anotando / rotulando-os.

Ao contravirtá-los externamente e ao fabricá-los ou construir pipelines de anotação complexos, você pode anotar em apenas algumas linhas de código usando ipyannotate.

Ele fornece widgets Jupyter projetados especificamente para anotação de dados.

Execute o seguinte comando para instalar:

```python
pip install ipyannotate

jupyter nbextension enable --py --sys-prefix ipyannotate
```

A anotação de dados fica mais fácil clicando em um botão. Portanto, o ipyannotate permite que você anexie rótulos de dados aos botões.

Suponha que temos algumas imagens de gatos e cães (não marcados). Podemos criar um pipeline de anotação como mostrado abaixo:

![Nenhum](https://miro.medium.com/v2/resize:fit:700/0*Ky69TzmvId4zeG4y)

Como mostrado acima, você pode anotar seus dados simplesmente clicando no botão correspondente.

Além disso, você pode recuperar os rótulos e usá-los conforme necessário no pipeline de dados.

**3\. Visualizando documentação em Jupyter** Ao trabalhar no Jupyter, é comum esquecer os parâmetros da função e referir-se à documentação oficial (ou StackOverflow). No entanto, você pode visualizar a documentação diretamente no caderno.

Pressione Shift-Tab para abrir o painel de documentação. Isso é muito útil e economiza tempo porque você não precisa abrir a documentação oficial sempre.

Aqui está uma demonstração:

![Nenhum](https://miro.medium.com/v2/resize:fit:700/0*Z8c9caAMPEdmo0dH)

Esse recurso também se aplica às suas funções personalizadas.

**4\. Receber notificações após executar uma célula de juppyter** Depois de executar algum código em uma célula Jupyter, normalmente mudamos para outras tarefas. Aqui, as pessoas geralmente precisam continuar mudando de volta para a guia Jupyter para verificar se a célula foi executada.

Para evitar isso, você pode usar o comando %%notify da extensão de jupyternotify.

Como o nome sugere, ele notifica o usuário por meio de uma notificação do navegador quando a célula Jupyter é concluída (sucesso ou falha).

Para instalá-lo, execute o seguinte comando:

```python
pip install jupyternotify
```

Em seguida, carregue a extensão:

```python
%load_ext jupyternotify
```

Tudo pronto!

Agora, sempre que você quiser receber notificações, basta digitar o seguinte comando mágico no topo da célula:

```python
%%notify
```

Você receberá a seguinte notificação sempre que a célula terminar em execução:

![Nenhum](https://miro.medium.com/v2/resize:fit:700/0*2pZ-LbM-GL8acdEB)

Clicar na notificação irá levá-lo de volta para a aba Jupyter.

**5\. Cortar a saída de células ao correr no Jupyter Notebook** Ao usar Jupyter, muitas vezes imprimimos muitos detalhes para acompanhar o progresso do código. No entanto, isso pode ser frustrante quando o painel de saída acumula muitos detalhes, e só estamos interessados na saída mais recente.

Além disso, ter que rolar para a parte inferior da saída sempre pode ser irritante.

Para limpar a saída de uma célula, você pode usar o método clear\_output do pacote IPython.

O IPython é pré-instalado com Python, portanto, nenhuma instalação é necessária.

Você pode importar o método como mostrado abaixo:

```python
from IPython.display import clear_output
```

Quando chamado, ele removerá a saída atual da célula, permitindo que você imprima os detalhes mais recentes depois.

Aqui está uma demonstração:

![Nenhum](https://miro.medium.com/v2/resize:fit:700/0*ri1VvifqEOWzi4LN)

Como demonstrado acima, vemos apenas a saída mais recente na célula. A produção anterior foi apagada.

**Sumário** Hoje, compartilhei algumas dicas incríveis de Jupyter. Acredito que essas dicas melhorarão sua eficiência de programação em Python.

Obrigado por ler.