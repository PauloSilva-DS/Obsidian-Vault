---
title: "Curso de Streamlit – Como Criar Apps e Sites com Streamlit"
source: "https://www.hashtagtreinamentos.com/streamlit-python"
author:
  - "[[Hashtag Treinamentos]]"
published: 2025-03-18
created: 2025-05-20
description: "Aprenda a criar aplicativos e sites com o nosso Curso de Streamlit! Dê seus primeiros passos com essa poderosa biblioteca e crie seu próprio aplicativo!"
tags:
  - "clippings"
---
### 🎉 SEMANA DO CONSUMIDOR

Últimos dias para comprar os cursos com 50% de desconto

[Ver detalhes](https://www.hashtagtreinamentos.com/pg-inscricao-semana-consumidor?src=lsc-site-banner&utm_source=site&utm_medium=banner&utm_campaign=geral&conversion=lcto-lsc02-mar25)

---

Postado em em 18 de março de 2025

Aprenda a criar aplicativos e sites com o nosso Curso de Streamlit! Dê seus primeiros passos com essa poderosa biblioteca e crie seu próprio aplicativo!

O objetivo deste curso é que você aprenda o que é o Streamlit, para que ele serve, e quais são as principais etapas para construir um aplicativo.

Vamos desenvolver dois projetos práticos para que você possa: criar seu próprio aplicativo de verificação de preços de ações e criar um aplicativo de chat.

Para o primeiro projeto, construiremos toda a interface do Streamlit, integraremos outras bibliotecas Python, e finalizaremos com um aplicativo completo que exibe um dashboard profissional de uma corretora de ações!

![Ícone Python](https://www.hashtagtreinamentos.com/wp-content/themes/hashtag/desenvolvimento_hashtag/assets/imgs/Todos-Cursos/%C3%8Dcones/python.svg) **Python** Impressionador

**Você vai aprender a linguagem de Programação que mais cresce no Mundo para fazer automações incríveis,** desenvolver sites, fazer análises de dados, trabalhar com Ciência de Dados, Inteligência Artificial, mesmo que você nunca tenha tido nenhum contato com Programação na vida.

[Começar agora](https://www.hashtagtreinamentos.com/curso-python?origemurl=hashtag_site_org_blog_banner_streamlit-python&utm_source=site-org&utm_medium=blog-banner&utm_campaign=programacao&utm_content=streamlit-python&conversion=perpetuo-les)

![Ícone do Python usado como fundo](https://www.hashtagtreinamentos.com/wp-content/themes/hashtag/desenvolvimento_hashtag/assets/imgs/Todos-Cursos/fundo-python.png) ![Três imagens de aulas do curso de Python](https://www.hashtagtreinamentos.com/wp-content/themes/hashtag/desenvolvimento_hashtag/assets/imgs/Todos-Cursos/telas-python.png) ![](https://www.hashtagtreinamentos.com/wp-content/themes/hashtag/desenvolvimento_hashtag/assets/imgs/Todos-Cursos/luz-inteira.png)

## Curso de Streamlit – Aula 1 – Como Funciona Criar Apps e Sites com o Streamlit

Vamos iniciar o Curso de Streamlit para que você possa sair do zero com essa biblioteca essencial do Python para criar aplicativos e sites!

Caso prefira esse conteúdo no formato de vídeo-aula, assista ao vídeo abaixo ou acesse o nosso [canal do YouTube](https://youtube.com/hashtagprograma%C3%A7%C3%A3o)!

****Para receber por e-mail o(s) arquivo(s) utilizados na aula, preencha**:**

Não vamos te encaminhar nenhum tipo de SPAM! A Hashtag Treinamentos é uma empresa preocupada com a proteção de seus dados e realiza o tratamento de acordo com a Lei Geral de Proteção de Dados (Lei n. 13.709/18). Qualquer dúvida, nos contate.

A primeira aula é uma introdução às funcionalidades e principais usos do Streamlit, uma ferramenta amplamente utilizada para criar aplicativos e sites, especialmente na área de ciência de dados.

Ao longo do curso, vamos desenvolver um aplicativo para acompanhamento de resultados de uma carteira de ações, semelhante aos sistemas utilizados em corretoras.

O curso será dividido em quatro etapas, para que você aprenda de forma progressiva a construir o aplicativo, integrar diferentes bibliotecas, criar o dashboard, e exibi-lo no Streamlit.

Vamos começar explorando o que é o Streamlit, para que ele serve, e quais são as principais etapas para construir um aplicativo. Desenvolveremos a interface, integraremos com a biblioteca do Yahoo Finance, e criaremos o gráfico para a aplicação.

### O que é o Streamlit?

O **Streamlit** é uma biblioteca Python de código aberto que permite aos desenvolvedores criar rapidamente aplicativos web interativos e visualizações de dados.

Essa ferramenta é especialmente popular entre cientistas de dados e engenheiros de machine learning, pois transforma scripts em Python em aplicativos funcionais, sem exigir um conhecimento profundo em **desenvolvimento web**.

O Streamlit se destaca por sua **simplicidade** e **eficiência**. Com poucas linhas de código, é possível criar interfaces de usuário interativas, gráficos dinâmicos e dashboards completos.

A biblioteca também é altamente compatível com outras ferramentas e bibliotecas de análise de dados, como **Pandas**, **Matplotlib** e **Plotly**, permitindo uma integração perfeita em projetos de .

Para mais detalhes, confira a [documentação oficial do Streamlit](https://docs.streamlit.io/). Ela é uma fonte valiosa para compreender profundamente cada uma das funcionalidades dessa biblioteca.

### Instalação do Streamlit

Para começar a usar o Streamlit, primeiro você precisa instalá-lo. Isso pode ser feito com o comando **pip install streamlit** no terminal do seu editor de código.

![](https://www.hashtagtreinamentos.com/wp-content/uploads/2024/08/Streamlit-2.png.webp)

Não vamos te encaminhar nenhum tipo de SPAM! A Hashtag Treinamentos é uma empresa preocupada com a proteção de seus dados e realiza o tratamento de acordo com a Lei Geral de Proteção de Dados (Lei n. 13.709/18). Qualquer dúvida, nos contate.

Além do Streamlit, todas as dependências necessárias serão instaladas automaticamente.

A instalação inclui bibliotecas importantes como o **Pandas**, essencial para manipulação e análise de dados dentro dos aplicativos criados com o Streamlit.

Para este curso é muito importante ter conhecimento prévio na biblioteca Pandas, pois muitas operações realizadas no Streamlit dependem dela para filtrar tabelas e realizar análises.

Caso você ainda não tenha familiaridade com o Pandas, recomendo as seguintes aulas:

- [Introdução ao Pandas – Saia do Zero em Uma Aula](https://www.hashtagtreinamentos.com/introducao-ao-pandas-python)
- [Pandas Python – O que é, para que Serve e Como Instalar](https://www.hashtagtreinamentos.com/pandas-python)

### Estrutura Básica de um Projeto com Streamlit

Um projeto típico com Streamlit segue uma estrutura básica:

1. **Importar as Bibliotecas**: Importar o próprio Streamlit, Pandas para manipulação de dados, e outras bibliotecas necessárias para análise ou visualização.
2. **Criar Funções para Carregamento de Dados**: Carregar os dados a partir de fontes externas como arquivos CSV, bancos de dados ou APIs.
3. **Preparar as Visualizações**: Transformar os dados brutos em formas gráficas ou tabulares que sejam fáceis de interpretar e analisar.
4. **Criar a Interface do Usuário**: Utilizar o Streamlit para exibir textos, gráficos, tabelas e outros elementos interativos na tela.

São esses os passos que seguiremos para criar nosso aplicativo.

### Biblioteca Yahoo Finance

Para obter os dados das cotações que exibiremos no aplicativo, utilizaremos a biblioteca do **Yahoo Finance**.

Essa biblioteca não é instalada automaticamente com o Streamlit, então precisamos instalá-la com o comando **pip install yfinance** no terminal.

![](https://www.hashtagtreinamentos.com/wp-content/uploads/2024/08/Streamlit-3.png.webp)

Não vamos te encaminhar nenhum tipo de SPAM! A Hashtag Treinamentos é uma empresa preocupada com a proteção de seus dados e realiza o tratamento de acordo com a Lei Geral de Proteção de Dados (Lei n. 13.709/18). Qualquer dúvida, nos contate.

**Leia também:**[Como pegar Cotações de Ações do Yahoo Finance com Python](https://www.hashtagtreinamentos.com/como-pegar-cotacoes-de-acoes-python)

### Importando as Bibliotecas Necessárias

Para iniciar o projeto, vamos importar a biblioteca do Streamlit usando o alias **st**, para facilitar seu uso ao longo do código.

Além dela, importaremos o Pandas para a manipulação de dados e o Yahoo Finance para obter as cotações das ações.

```
import streamlit as st
import pandas as pd
import yfinance as yf
```

### Carregamento de Dados – Yahoo Finance

Após importar as bibliotecas, a próxima etapa é construir a função responsável por carregar os dados que serão analisados e visualizados.

Utilizaremos a biblioteca **yfinance** para obter as cotações históricas de ações, especificamente do Itaú (**ITUB4.SA**), entre 2010 e 2024. Esses dados serão fundamentais para criar as visualizações no aplicativo.

Vamos definir a função **carregar\_dados()**. Ela recebe o parâmetro **empresa**, que será o **ticker** (código) da ação que desejamos consultar.

Dentro da função, criaremos um objeto **Ticker** da biblioteca yfinance usando o código da ação da empresa. É importante que o código do ticker seja informado corretamente para evitar confusões com outras bolsas de valores.

O Yahoo Finance obtém informações de várias bolsas ao redor do mundo. Como queremos dados da bolsa de valores brasileira, é essencial incluir o sufixo.SA ao final do código, como em ITUB4.SA.

A partir do objeto criado, podemos acessar várias informações sobre a ação. Para consultar o histórico de preços, utilizaremos o método **history**, que permite personalizar a busca com diversos parâmetros. Para o nosso projeto, utilizaremos:

- **period**: Define o período de tempo desejado. Vamos utilizar **period=’1d’** para obter dados diários.
- **start**: Data de início no formato **YYYY-MM-DD**. Vamos buscar dados a partir de 1 de janeiro de 2000.
- **end**: Data final no mesmo formato. Vamos buscar dados até 1 de julho de 2024.

O resultado será um **DataFrame** contendo informações como preço de abertura, fechamento, máximo, mínimo e volume de negociação para cada dia dentro do intervalo.

Podemos visualizar esse DataFrame com o seguinte código:

```
import streamlit as st
import yfinance as yf
import pandas as pd

dados_acao = yf.Ticker("ITUB4.SA")
dados = dados_acao.history(period='1d', start='2000-01-01', end='2024-07-01')
print(dados)
```
![dataframe do método history](https://www.hashtagtreinamentos.com/wp-content/uploads/2024/08/Streamlit-4.png)

dataframe do método history

Como queremos apenas o preço de fechamento das ações, podemos filtrar o DataFrame para obter apenas a coluna Close.

Para obter os dados no formato de um DataFrame pandas com apenas a coluna “Close”, utilizamos a notação de **dois colchetes** na seleção, o que indica que estamos passando uma lista contendo uma única coluna.

Feito isso, podemos retornar o DataFrame resultante, que conterá apenas os preços de fechamento da ação ao longo desse período, com as datas como índice.

Dessa forma, nossa função **carregar\_dados** ficará assim:

```
def carregar_dados(empresa):
    dados_acao = yf.Ticker(empresa)
    cotacoes_acao = dados_acao.history(period='1d', start='2000-01-01', end='2024-07-01')
    cotacoes_acao = cotacoes_acao[["Close"]]
    return cotacoes_acao
```

### Decorator no Python – Cache

Na primeira execução, o tempo necessário para carregar os dados pela função **carregar\_dados()** pode variar, dependendo da conexão à internet do usuário.

Para otimizar esse processo e garantir que as próximas execuções sejam mais rápidas, podemos utilizar o mecanismo de **cache** com o decorator **@st.cache** \_data do Streamlit.

Ao utilizar esse decorator, o Streamlit armazena o resultado da função após a primeira vez que ela é chamada com um conjunto específico de argumentos.

Nas chamadas subsequentes, com os mesmos argumentos, o Streamlit retorna o resultado armazenado, sem precisar executar a função novamente.

Isso melhora o desempenho do aplicativo, tornando sua execução mais rápida. Caso haja uma mudança na função ou nos argumentos, o cache é invalidado e a função é executada novamente.

O cache permanece ativo enquanto a aplicação estiver em execução, melhorando a eficiência, minimizando chamadas repetidas a APIs externas e proporcionando uma experiência mais ágil para o usuário.

```
import streamlit as st
import yfinance as yf
import pandas as pd

@st.cache_data
def carregar_dados(empresa):
    dados_acao = yf.Ticker(empresa)
    cotacoes_acao = dados_acao.history(period='1d', start='2000-01-01', end='2024-07-01')
    cotacoes_acao = cotacoes_acao[["Close"]]
    return cotacoes_acao
```

**Leia também**: [Decorators no Python – Para que serve o @ no Python?](https://www.hashtagtreinamentos.com/decorators-no-python)

### Preparar as Visualizações

Após carregar os dados, o próximo passo é prepará-los para a visualização. Como já definimos a coluna relevante para o nosso aplicativo, podemos visualizar o resultado em forma de DataFrame para conferência.

```
@st.cache_data
def carregar_dados(empresa):
    dados_acao = yf.Ticker(empresa)
    cotacoes_acao = dados_acao.history(period='1d', start='2000-01-01', end='2024-07-01')
    cotacoes_acao = cotacoes_acao[["Close"]]
    return cotacoes_acao

dados  = carregar_dados("ITUB4.SA")
print(dados)
```
![Dataframe apenas com os preços de fechamento](https://www.hashtagtreinamentos.com/wp-content/uploads/2024/08/Streamlit-5.png)

Dataframe apenas com os preços de fechamento

O resultado exibirá corretamente apenas as colunas de data e o preço de fechamento. Dessa forma, temos os dados preparados para a visualização gráfica.

### Desenvolvimento da Interface e Criação do Gráfico

Com todas as etapas concluídas, podemos partir para a criação da interface visual do aplicativo.

Por enquanto, criaremos uma interface básica que será aprimorada ao longo das próximas aulas.

Para adicionar um texto na tela usando o Streamlit, podemos usar a função **write()**. Essa função é versátil e pode renderizar diversos tipos de conteúdo, como strings, markdown, DataFrames, objetos, gráficos, imagens, e muito mais.

Ela consegue interpretar automaticamente o tipo de conteúdo que você está passando e renderizá-lo adequadamente. Para o nosso projeto, utilizaremos a renderização de **Markdown** para formatar e editar o nosso texto.

Se você quiser saber mais sobre Markdown e como essa linguagem de marcação funciona, confira a aula: [Markdown para formatação de textos](https://www.hashtagtreinamentos.com/markdown-python).

Basicamente, a interface do nosso aplicativo será um texto informando que o gráfico abaixo representa a evolução do preço das ações do Itaú, seguido de um gráfico de linha mostrando essa evolução, e um texto de encerramento.

Para construir o gráfico de linhas, utilizaremos o método **st.line\_chart()**, passando para ele o DataFrame dados obtido pela função carregar\_dados().

Esse método automaticamente mapeia os dados fornecidos e renderiza um gráfico de linha, exibindo-o diretamente na interface do Streamlit.

As linhas do gráfico serão traçadas de acordo com os valores ao longo do eixo X (geralmente correspondendo ao índice do DataFrame) e o eixo Y (com os valores das colunas).

Este gráfico será interativo, o que significa que os usuários podem passar o mouse sobre ele para ver os valores exatos dos pontos de dados.

Incluindo a interface com o gráfico, o código final do nosso aplicativo, nesta primeira aula, ficará assim:

```
# importar as bibliotecas
import streamlit as st
import yfinance as yf
import pandas as pd

# criar as funções de carregamento de dados
@st.cache_data
def carregar_dados(empresa):
    dados_acao = yf.Ticker(empresa)
    cotacoes_acao = dados_acao.history(period='1d', start='2000-01-01', end='2024-07-01')
    cotacoes_acao = cotacoes_acao[["Close"]]
    return cotacoes_acao

# preparar as visualizações
dados  = carregar_dados("ITUB4.SA")

# criar a interface do streamlit
st.write("""
# App Preço de Ações
O gráfico abaixo representa a evolução do preço das ações do Itaú (ITUB4) ao longo dos anos
""")

# criar o gráfico
grafico = st.line_chart(dados)

st.write("""
# Fim do app
""")
```

### Testes e Execução do Aplicativo com Streamlit Python

Para executar o aplicativo localmente e verificar seu funcionamento, é necessário executá-lo pelo terminal do editor de código.

Certifique-se de que o caminho indicado no seu terminal seja a pasta onde está localizado o arquivo no qual você está desenvolvendo o código. No meu caso, é a pasta **curso\_streamlit**, onde está o arquivo **main.py**.

Feita essa verificação, para executar o aplicativo, basta usar o comando **streamlit run main.py**.

![](https://www.hashtagtreinamentos.com/wp-content/uploads/2024/08/Streamlit-6.png.webp)

Não vamos te encaminhar nenhum tipo de SPAM! A Hashtag Treinamentos é uma empresa preocupada com a proteção de seus dados e realiza o tratamento de acordo com a Lei Geral de Proteção de Dados (Lei n. 13.709/18). Qualquer dúvida, nos contate.

**Obs.:** Caso seu arquivo tenha outro nome, substitua main.py pelo nome do seu arquivo.

Executando esse comando, uma aba será aberta no seu navegador com o aplicativo no Streamlit exibindo o texto e o gráfico definido.

![Tela do Aplicativo no Streamlit](https://www.hashtagtreinamentos.com/wp-content/uploads/2024/08/Streamlit-7.png)

Tela do Aplicativo no Streamlit

A partir de agora, qualquer mudança ou ajuste feito no código pode ser visualizado em tempo real. Basta acessar essa aba, clicar no menu no canto superior direito e selecionar **Rerun**.

![Rerun streamlit](https://www.hashtagtreinamentos.com/wp-content/uploads/2024/08/Streamlit-8.png)

Rerun streamlit

Assim, você poderá visualizar imediatamente o funcionamento e as modificações feitas.

Para interromper a execução do aplicativo, abra o terminal do seu editor de código e pressione **Ctrl + C**. Com isso, concluímos o objetivo inicial do Curso de Streamlit, que é desenvolver um aplicativo desde a instalação das ferramentas até a criação da interface interativa e otimizada para a visualização de dados.

*[Voltar ao índice](https://www.hashtagtreinamentos.com/#rank-math-toc)*

## Curso de Streamlit – Aula 2 – Filtros e Gráficos

Vamos seguir para a segunda aula do Curso de Streamlit no Python, onde você aprenderá a criar um aplicativo do zero que permite visualizar várias ações de forma gráfica em um único lugar!

Caso prefira esse conteúdo no formato de vídeo-aula, assista ao vídeo abaixo ou acesse o nosso [canal do YouTube](https://youtube.com/hashtagprograma%C3%A7%C3%A3o)!

****Para receber por e-mail o(s) arquivo(s) utilizados na aula, preencha**:**

Não vamos te encaminhar nenhum tipo de SPAM! A Hashtag Treinamentos é uma empresa preocupada com a proteção de seus dados e realiza o tratamento de acordo com a Lei Geral de Proteção de Dados (Lei n. 13.709/18). Qualquer dúvida, nos contate.

Nesta aula, o foco será desenvolver uma interface que possibilite a seleção de múltiplos ativos, permitindo a visualização de mais de um gráfico simultaneamente na tela da sua aplicação.

Dessa forma, o usuário poderá escolher quais ações deseja ou não visualizar no gráfico exibido pelo aplicativo.

Além disso, você aprenderá como ativar e utilizar o modo escuro (dark mode) no seu aplicativo no Streamlit. O dark mode oferece um visual mais agradável em certos casos e é uma preferência comum entre os usuários, pois reduz a fadiga ocular.

### Configuração de Tema – Dark Mode

Vamos começar com a personalização da aparência da sua aplicação no Streamlit. A criação de um modo escuro é possível através da configuração do arquivo **config.toml**.

Caso não exista um arquivo específico para as configurações visuais, o Streamlit usará as configurações padrão.

Esse arquivo permite definir cores e temas personalizados para a aplicação, proporcionando uma experiência mais agradável para o usuário.

O primeiro passo é criar o arquivo config.toml, que deve ser colocado dentro de uma pasta chamada **.streamlit**, localizada no diretório onde estão os arquivos do projeto.

![Configuração de Tema – Dark Mode](https://www.hashtagtreinamentos.com/wp-content/uploads/2024/08/Streamlit-Aula-2-1.png.webp)

Configuração de Tema – Dark Mode

O formato **TOML** (Tom’s Obvious Minimal Language) é utilizado para definir as configurações desse arquivo. É uma linguagem simplificada, amplamente utilizada em sistemas para configurações visuais básicas.

Você pode aprender mais sobre essa linguagem na [documentação dela](https://toml.io/pt/v1.0.0), mas a estrutura é bastante simples e intuitiva, facilitando sua implementação no projeto.

Dentro desse arquivo, o nome da seção que estamos configurando é colocado entre colchetes, e abaixo dela, o parâmetro com o valor atribuído.

Para aplicar o Dark Mode, basta configurar a seção \[theme\] com o parâmetro base=”dark”.

```
[theme]
base="dark"
```

Após essa configuração, é necessário pausar e reiniciar o projeto com o comando **streamlit run main.py** para que o tema seja aplicado.

### Carregar Vários Tickers de Ações

Para que o usuário possa selecionar e visualizar as ações de diferentes empresas dentro do aplicativo, precisamos ajustar a função **carregar\_dados()**.

Anteriormente, essa função esperava um único ticker para carregar os dados de uma ação específica. Agora, ela receberá uma lista de **tickers**.

Para processar e obter os dados de múltiplos tickers, alteramos o objeto yf.Ticker para **yf.Tickers**.

Essa classe da biblioteca yfinance permite trabalhar com um ou mais tickers de ações simultaneamente. Ela recebe uma **string** contendo os códigos de cada ação, separados por espaços.

Utilizaremos a função **join** do Python para converter a lista de tickers recebida pela função carregar\_dados() em uma única string com os códigos separados por espaço.

Em seguida, o método **.history** será chamado no objeto Tickers, retornando um DataFrame de multi-índice no Pandas com o histórico de preços de todos os ativos especificados.

Para filtrar apenas o preço de fechamento de cada um dos ativos, acessamos a coluna Close no DataFrame resultante.

```
# importar as bibliotecas
import streamlit as st
import yfinance as yf
import pandas as pd

# criar as funções de carregamento de dados
@st.cache_data
def carregar_dados(empresas):
    texto_tickers = " ".join(empresas)
    dados_acao = yf.Tickers(texto_tickers)
    cotacoes_acao = dados_acao.history(period='1d', start='2000-01-01', end='2024-07-01')
    cotacoes_acao = cotacoes_acao["Close"]
    return cotacoes_acao
```

Com a função modificada, podemos definir a lista de ações que desejamos disponibilizar para o usuário e passá-la como argumento para a função carregar\_dados().

```
# importar as bibliotecas
import streamlit as st
import yfinance as yf
import pandas as pd

# criar as funções de carregamento de dados
@st.cache_data
def carregar_dados(empresas):
    texto_tickers = " ".join(empresas)
    dados_acao = yf.Tickers(texto_tickers)
    cotacoes_acao = dados_acao.history(period='1d', start='2000-01-01', end='2024-07-01')
    cotacoes_acao = cotacoes_acao["Close"]
    return cotacoes_acao

# preparar as visualizações
acoes = ["ITUB4.SA", "PETR4.SA", "MGLU3.SA", "VALE3.SA", "ABEV3.SA", "GGBR4.SA"]
dados = carregar_dados(acoes)

# criar a interface do streamlit
st.write("""
# App Preço de Ações
O gráfico abaixo representa a evolução do preço das ações do Itaú (ITUB4) ao longo dos anos
""")

# criar o gráfico
grafico = st.line_chart(dados)

st.write("""
# Fim do app
""")
```

Com essas alterações, o aplicativo no Streamlit exibirá um gráfico com todas as ações selecionadas.

![gráfico com todas as ações selecionadas](https://www.hashtagtreinamentos.com/wp-content/uploads/2024/08/Streamlit-Aula-2-2.png)

gráfico com todas as ações selecionadas

### Implementação de Filtros – Multi-select

Para que o usuário possa selecionar as ações que deseja visualizar no gráfico, utilizaremos o widget **Multi-select** do Streamlit. Esse widget cria um menu onde o usuário pode escolher as ações que serão exibidas.

Esse menu será composto por todas as colunas do DataFrame **dados**, correspondentes às ações carregadas pela função carregar\_dados().

Ao criar esse widget, precisamos passar dois argumentos: uma string, que será o texto exibido para o usuário, e os dados que poderão ser selecionados.

Faremos isso de forma dinâmica, listando as colunas presentes no DataFrame dados. Isso garante que apenas opções válidas e presentes nos dados carregados sejam exibidas.

```
lista_acoes = st.multiselect("Escolha as ações para visualizar", dados.columns)
```

Isso criará uma lista de opções onde o usuário poderá selecionar uma ou mais ações, e as ações escolhidas serão armazenadas na variável **lista\_acoes**.

Quando uma ação é selecionada no Multi-select, ela atualiza automaticamente a lista de ações escolhidas pelo usuário, proporcionando uma experiência interativa e fluida.

Com as ações selecionadas e armazenadas, podemos filtrar o DataFrame dados para exibir apenas as colunas correspondentes às ações escolhidas. Isso é feito utilizando uma [condicional if](https://www.hashtagtreinamentos.com/if-no-python).

```
# preparar as visualizações
lista_acoes = st.multiselect("Escolha as ações para visualizar", dados.columns)
if lista_acoes:
    dados = dados[lista_acoes]
```

Se houver ações na **lista\_acoes**, o DataFrame dados será filtrado para exibir apenas as colunas dessas ações, que o Streamlit utilizará para gerar o gráfico. Caso contrário, se nenhuma ação for selecionada, o gráfico não será gerado.

Além disso, adicionaremos uma segunda verificação para o caso de o usuário selecionar apenas uma ação. Isso é necessário para que o Streamlit consiga exibir o gráfico corretamente.

Quando temos uma única ação selecionada, o nome da coluna será o mesmo que o ticker da ação, o que faz com que o Streamlit espere um segundo ticker para exibir as informações no gráfico.

Para resolver essa incompatibilidade, verificaremos se há apenas uma ação selecionada. Se sim, a coluna dessa ação será renomeada para Close.

```
# preparar as visualizações
lista_acoes = st.multiselect("Escolha as ações para visualizar", dados.columns)
if lista_acoes:
    dados = dados[lista_acoes]
    if len(lista_acoes) == 1:
        acao_unica = lista_acoes[0]
        dados = dados.rename(columns={acao_unica: "Close"})
```

Dessa forma, o usuário poderá selecionar as ações que deseja visualizar, e os dados serão filtrados de maneira adequada.

### Ajustes na Interface do Streamlit e Organização do Código

Com os dados filtrados, finalmente podemos gerar o gráfico de linhas. Antes disso, faremos um ajuste no texto exibido no aplicativo para refletir a exibição de múltiplas ações, e removeremos a mensagem **“Fim do app”**, que agora se torna desnecessária.

Além disso, para facilitar a leitura e manutenção do código, é uma boa prática mover a etapa de preparar as visualizações para antes da geração do gráfico.

```
# importar as bibliotecas
import streamlit as st
import yfinance as yf
import pandas as pd

# criar as funções de carregamento de dados
@st.cache_data
def carregar_dados(empresas):
    texto_tickers = " ".join(empresas)
    dados_acao = yf.Tickers(texto_tickers)
    cotacoes_acao = dados_acao.history(period='1d', start='2000-01-01', end='2024-07-01')
    cotacoes_acao = cotacoes_acao["Close"]
    return cotacoes_acao

acoes = ["ITUB4.SA", "PETR4.SA", "MGLU3.SA", "VALE3.SA", "ABEV3.SA", "GGBR4.SA"]
dados = carregar_dados(acoes)

# criar a interface do streamlit
st.write("""
# App Preço de Ações
O gráfico abaixo representa a evolução do preço das ações ao longo dos anos
""")

# preparar as visualizações
lista_acoes = st.multiselect("Escolha as ações para visualizar", dados.columns)
if lista_acoes:
    dados = dados[lista_acoes]
    if len(lista_acoes) == 1:
        acao_unica = lista_acoes[0]
        dados = dados.rename(columns={acao_unica: "Close"})

# criar o gráfico
grafico = st.line_chart(dados)
```

Executando esse código, teremos nosso aplicativo funcionando conforme o esperado, com um menu interativo onde o usuário pode selecionar e remover as ações que deseja ou não visualizar.

![Exibição dos gráficos](https://www.hashtagtreinamentos.com/wp-content/uploads/2024/08/Streamlit-Aula-2-3.png)

Exibição dos gráficos

![Opção de selecionar ações no Streamlit](https://www.hashtagtreinamentos.com/wp-content/uploads/2024/08/Streamlit-Aula-2-4.png)

Opção de selecionar ações no Streamlit

*[Voltar ao índice](https://www.hashtagtreinamentos.com/#rank-math-toc)*

Na terceira aula do Curso de Streamlit no Python, você aprenderá a adicionar uma barra lateral com filtros para ações e um slider para selecionar o período de análise.

Caso prefira esse conteúdo no formato de vídeo-aula, assista ao vídeo abaixo ou acesse o nosso [canal do YouTube](https://youtube.com/hashtagprograma%C3%A7%C3%A3o)!

****Para receber por e-mail o(s) arquivo(s) utilizados na aula, preencha**:**

Não vamos te encaminhar nenhum tipo de SPAM! A Hashtag Treinamentos é uma empresa preocupada com a proteção de seus dados e realiza o tratamento de acordo com a Lei Geral de Proteção de Dados (Lei n. 13.709/18). Qualquer dúvida, nos contate.

Nesta aula, vamos implementar barras laterais (sidebars) no nosso aplicativo Streamlit. A sidebar será o local onde organizaremos o filtro de ações, desenvolvido na aula anterior, e também adicionaremos um slider para filtrar as datas, permitindo que o usuário selecione o período desejado para a análise.

### Implementação da Sidebar – Barra Lateral no Streamlit

A sidebar é um componente essencial para criar um aplicativo de visualização de dados que seja eficiente, organizado e com um visual profissional.

Ela aparece no lado esquerdo da tela e melhora significativamente a navegação e a usabilidade do seu aplicativo, permitindo que o usuário acesse filtros e configurações extras sem sobrecarregar a interface principal onde o gráfico é exibido.

Criar uma sidebar no Streamlit é bastante simples. Basta utilizar a propriedade **st.sidebar**, que pode ser atribuída a uma variável ou usada diretamente. Com ela, você pode adicionar textos, botões, sliders, e outros elementos diretamente na barra lateral.

O comando **st.sidebar** é utilizado para definir que os elementos subsequentes, como seletores multiselect e sliders, sejam posicionados na barra lateral.

Por exemplo, para adicionar um título à barra lateral, utilizamos **st.sidebar.header()**. Agora, vamos criar nossa barra lateral com o título “ **Filtros** ” e mover o seletor de ações para dentro dela.

Isso torna a interface mais organizada e permitirá que o usuário interaja com as opções de forma mais intuitiva.

Para adicionar o filtro de ações dentro da barra lateral, basta usar st.sidebar antes do filtro **multiselect**.

```
# criar a interface do streamlit
st.write("""
# App Preço de Ações
O gráfico abaixo representa a evolução do preço das ações ao longo dos anos
""")

# título barra lateral
barra_lateral = st.sidebar.header("Filtros")

# preparar as visualizações
# filtro de ações
lista_acoes = st.sidebar.multiselect("Escolha as ações para exibir no gráfico", dados.columns)
if lista_acoes:
    dados = dados[lista_acoes]
    if len(lista_acoes) == 1:
        acao_unica = lista_acoes[0]
        dados = dados.rename(columns={acao_unica: "Close"})
```

A partir do momento que utilizamos **st.sidebar** para criar ao menos um elemento, a barra lateral é gerada corretamente no aplicativo.

![Barra lateral](https://www.hashtagtreinamentos.com/wp-content/uploads/2024/08/Streamlit-Aula-3-1-1024x441.png.webp)

Barra lateral

Repare que os filtros de ações continuam funcionando normalmente; apenas mudamos sua posição na interface.

### Slider no Streamlit – Filtros de Datas

A seleção de datas é uma funcionalidade essencial quando estamos trabalhando com análises temporais.

O filtro de datas pode ser implementado de várias formas, como campos de input para seleção manual ou, como faremos aqui, um **slider** que permite escolher o intervalo por meio de um controle deslizante.

Para implementar um slider no Streamlit, basta usar o widget **st.sidebar.slider()**, de forma semelhante ao que fizemos com o multiselect. Para configurar o slider corretamente, você precisará definir os seguintes parâmetros:

- **label:** Uma string que será exibida como texto para o usuário.
- **min\_value:** A data mínima que pode ser selecionada, ou seja, a data mais antiga disponível nos dados.
- **max\_value:** A data máxima disponível nos dados.
- **value:** O intervalo inicial selecionado no slider, que configuraremos para começar com o intervalo completo, do início ao fim dos dados.
- **step** (opcional): O incremento do slider. No nosso caso, usaremos um incremento de um dia, permitindo que o usuário selecione datas com precisão diária.

O parâmetro step é opcional pois por padrão o incremento do slider já é feito de 1 em 1, então não precisaríamos declará-lo. Se desejar personalizar o incremento para um valor maior, será necessário importar a classe timedelta da biblioteca datetime.

```
import streamlit as st
import yfinance as yf
import pandas as pd
from datetime import timedelta
```

Antes de configurar o slider, precisamos extrair as datas mínima e máxima do índice do DataFrame **dados**.

Para que o Streamlit trabalhe adequadamente com essas datas, elas precisam estar no formato **datetime**, padrão do Python.

Como as datas obtidas pelo **yfinance** estão no formato **timestamp**, usaremos o método **.to\_pydatetime()** para convertê-las adequadamente.

```
data_inicial = dados.index.min().to_pydatetime()
data_final = dados.index.max().to_pydatetime()
```

**Leia também:**[Como trabalhar com Tempo no Python – Biblioteca Datetime](https://www.hashtagtreinamentos.com/como-trabalhar-com-tempo-no-python)

Com as datas inicial e final obtidas, podemos criar o nosso slider, atribuindo a data inicial ao parâmetro **min\_value** e a data final ao **max\_value**.

O parâmetro **value** recebe uma [tupla em Python](https://www.hashtagtreinamentos.com/tuplas-no-python) com o valor inicial e final do intervalo que o slider deve começar selecionando por padrão.

Por fim, para o parâmetro **step**, utilizaremos a classe **timedelta** para definir o incremento de um dia.

```
# preparar as visualizações
# filtro de ações
lista_acoes = st.sidebar.multiselect("Escolha as ações para exibir no gráfico", dados.columns)
if lista_acoes:
    dados = dados[lista_acoes]
    if len(lista_acoes) == 1:
        acao_unica = lista_acoes[0]
        dados = dados.rename(columns={acao_unica: "Close"})

#filtro de datas
data_inicial = dados.index.min().to_pydatetime()
data_final = dados.index.max().to_pydatetime()
intervalo_datas = st.sidebar.slider("Selecione o período", 
                                    min_value=data_inicial, 
                                    max_value=data_final, value=(data_inicial, data_final), step=timedelta(days=1))
```

Esse código já criará o slider dentro do aplicativo, mas ainda não filtrará as informações no gráfico.

![slider com datas no Streamlit](https://www.hashtagtreinamentos.com/wp-content/uploads/2024/08/Streamlit-Aula-3-2-1024x455.png.webp)

slider com datas no Streamlit

Agora que temos o slider configurado, vamos aplicar o filtro ao nosso DataFrame dados, para que o aplicativo exiba o gráfico de ações apenas dentro do período selecionado pelo usuário.

No caso do filtro de datas, sempre teremos um intervalo selecionado, então não precisamos lidar com situações em que nenhuma data foi escolhida.

Para filtrar as linhas do DataFrame, usaremos o método **.loc\[\]**.Esse método nos permite definir um intervalo que filtra as linhas do DataFrame a partir do índice.

Então, para determinar o período que desejamos exibir, basta definir o intervalo do.loc\[\] como as datas inicial e final selecionadas no **intervalo\_data**.

```
# importar as bibliotecas
import streamlit as st
import yfinance as yf
import pandas as pd
from datetime import timedelta

# criar as funções de carregamento de dados
@st.cache_data
def carregar_dados(empresas):
    texto_tickers = " ".join(empresas)
    dados_acao = yf.Tickers(texto_tickers)
    cotacoes_acao = dados_acao.history(period='1d', start='2000-01-01', end='2024-07-01')
    cotacoes_acao = cotacoes_acao["Close"]
    return cotacoes_acao

acoes = ["ITUB4.SA", "PETR4.SA", "MGLU3.SA", "VALE3.SA", "ABEV3.SA", "GGBR4.SA"]
dados = carregar_dados(acoes)

# criar a interface do streamlit
st.write("""
# App Preço de Ações
O gráfico abaixo representa a evolução do preço das ações ao longo dos anos
""")

# título barra lateral
barra_lateral = st.sidebar.header("Filtros")

# preparar as visualizações
# filtro de ações
lista_acoes = st.sidebar.multiselect("Escolha as ações para exibir no gráfico", dados.columns)
if lista_acoes:
    dados = dados[lista_acoes]
    if len(lista_acoes) == 1:
        acao_unica = lista_acoes[0]
        dados = dados.rename(columns={acao_unica: "Close"})

#filtro de datas
data_inicial = dados.index.min().to_pydatetime()
data_final = dados.index.max().to_pydatetime()
intervalo_datas = st.sidebar.slider("Selecione o período", 
                                    min_value=data_inicial, 
                                    max_value=data_final, value=(data_inicial, data_final), step=timedelta(days=1))
dados = dados.loc[intervalo_datas[0]:intervalo_datas[1]]

# criar o gráfico
grafico = st.line_chart(dados)
```

Com isso, o filtro de datas estará funcional dentro do aplicativo, permitindo ao usuário visualizar as ações apenas no período determinado.

![filtro com datas funcionando no streamlit](https://www.hashtagtreinamentos.com/wp-content/uploads/2024/08/Streamlit-Aula-3-3-1024x447.png.webp)

filtro com datas funcionando no streamlit

O uso de **sidebars**, **filtros de datas** e **sliders** é essencial para a criação de aplicações interativas e profissionais no Streamlit.

Esses componentes não apenas aprimoram a experiência do usuário, tornando a interface mais intuitiva e fácil de navegar, mas também aumentam a versatilidade da aplicação, permitindo que ela lide com diferentes tipos de dados e análises de forma dinâmica e eficiente.

*[Voltar ao índice](https://www.hashtagtreinamentos.com/#rank-math-toc)*

## Curso de Streamlit – Aula 4 – Dashboard Completo de Corretora de Ações

Chegamos à quarta e última aula do nosso Curso de Streamlit, onde vamos finalizar a construção do aplicativo que exibe preços de ações, incluindo resumos de performance e gráficos interativos.

Caso prefira esse conteúdo no formato de vídeo-aula, assista ao vídeo abaixo ou acesse o nosso [canal do YouTube](https://youtube.com/hashtagprograma%C3%A7%C3%A3o)!

****Para receber por e-mail o(s) arquivo(s) utilizados na aula, preencha**:**

Não vamos te encaminhar nenhum tipo de SPAM! A Hashtag Treinamentos é uma empresa preocupada com a proteção de seus dados e realiza o tratamento de acordo com a Lei Geral de Proteção de Dados (Lei n. 13.709/18). Qualquer dúvida, nos contate.

Na aula de hoje, concluiremos o projeto do aplicativo para verificação de preços de ações.

Ao final dessa aula, você estará capacitado a desenvolver um dashboard completo para análise de preços de ações, integrando dados financeiros em tempo real e calculando a performance de ações e carteiras.

Tudo isso será feito em uma interface atraente e intuitiva, proporcionando uma experiência agradável e prática para que o usuário possa utilizar o aplicativo com facilidade.

### Obtendo Lista Completa de Ações – Dados Ibovespa

O primeiro passo é implementar a capacidade de obter uma lista completa de ações de acordo com os dados da Ibovespa.

Essa lista de tickers pode ser obtida de várias formas: pesquisando manualmente no Google por “ **índice Ibovespa Tickers** ” ou até mesmo via [web scraping](https://www.hashtagtreinamentos.com/web-scraping-python).

Para facilitar o desenvolvimento, disponibilizei um arquivo CSV com a lista completa no material desta aula. Baixe essa planilha e salve-a na mesma pasta onde está desenvolvendo o aplicativo.

Em seguida, vamos carregar os dados dessa planilha para o código usando o Pandas. Criaremos a função **carregar\_tickers\_acoes()** logo abaixo da função carregar\_dados().

Essa função vai carregar e ler o arquivo CSV chamado **IBOV.csv** usando o Pandas, com o separador definido como ponto e vírgula (**sep=”;”**). O DataFrame resultante será armazenado na variável **base\_tickers**.

A partir daí, vamos extrair a coluna Código e convertê-la em uma lista de strings, para ser utilizada na função **carregar\_dados**.

Para garantir que o yfinance reconheça corretamente os tickers, vamos adicionar o sufixo **.SA** a cada ticker na lista, identificando assim as ações da bolsa de valores brasileira (**B3**) no Yahoo Finance. Essa etapa será feita usando **List Comprehension**.

Se você tiver dúvidas sobre List Comprehension, recomendo assistir à aula: [O que é List Comprehension no Python e como usá-lo](https://www.hashtagtreinamentos.com/list-comprehension-python).

Assim como na função carregar\_dados, vamos adicionar o decorator **@st.cache\_data** antes dessa nova função.

```
@st.cache_data
def carregar_tickers_acoes():
    base_tickers = pd.read_csv("IBOV.csv", sep=";")
    tickers = list(base_tickers["Código"])
    tickers = [item + ".SA" for item in tickers]
    return tickers
```

Com esses ajustes, a função retornará uma lista com todos os tickers da Ibovespa. Agora, em vez de declarar manualmente a lista de tickers na variável **acoes**, podemos simplesmente chamar essa função.

```
acoes = carregar_tickers_acoes()
dados = carregar_dados(acoes)
```

Feitas essas alterações, ao executar o aplicativo, teremos uma lista completa de tickers que podem ser selecionados.

![Todas as ações disponíveis](https://www.hashtagtreinamentos.com/wp-content/uploads/2024/08/streamlit-aula-4-1-1024x493.png.webp)

Todas as ações disponíveis

### Cálculo e Exibição da Performance de Ativos

Agora, vamos construir a seção responsável por exibir a performance dos ativos dentro do aplicativo. Essa seção ficará localizada logo abaixo do gráfico.

Primeiro, definiremos a variável **texto\_performance\_ativos** como uma string vazia. Ela será usada para armazenar os resultados da performance de cada ativo.

Em seguida, verificaremos quantas ações foram selecionadas. Se nenhuma ação for selecionada pelo usuário, a **lista\_acoes** será automaticamente preenchida com todas as colunas disponíveis no DataFrame dados, ou seja, todas as ações carregadas.

Se apenas uma ação for escolhida, a coluna que contém os preços de fechamento será renomeada para Close, facilitando o manuseio e a visualização.

```
# calculo de perfomance
texto_performance_ativos = ""

if len(lista_acoes)==0:
    lista_acoes = list(dados.columns)
elif len(lista_acoes)==1:
    dados = dados.rename(columns={"Close": acao_unica})
```

Agora, partiremos para o cálculo da performance dos ativos. A performance de um ativo é calculada como o valor final do ativo dividido pelo valor inicial, subtraindo 1.

**Performance = (Valor Final / Valor Inicial) – 1**

Para calcular a performance de cada ativo na lista\_acoes, criaremos um **loop** **for** para percorrer cada um desses tickers.

Durante o loop, calcularemos a performance do ativo dividindo o preço de fechamento mais recente (**iloc\[-1\]**) pelo preço de fechamento no início do período (**iloc\[0\]**).

Em seguida, subtrairemos 1 dessa divisão e converteremos o valor obtido para um número **float**.

```
for acao in lista_acoes:
    performance_ativo = dados[acao].iloc[-1] / dados[acao].iloc[0] - 1
    performance_ativo = float(performance_ativo)
```

Depois, formatamos os resultados de cada ativo e os adicionamos à string **texto\_performance\_ativos**.

Caso a performance seja positiva (crescimento), o resultado será exibido em verde. Para performance negativa (queda), o resultado será exibido em vermelho. Se a performance for neutra, sem variação, o resultado será exibido sem cor adicional.

Como estamos utilizando Markdown para estilizar e formatar os textos na aplicação, usaremos a notação **:cor\[Texto desejado\]** para alterar as cores.

Para que o caractere especial **\\n** funcione corretamente no Markdown, fazendo o texto subsequente ser exibido na linha abaixo, é necessário adicionar dois espaços antes dele.

```
for acao in lista_acoes:
    performance_ativo = dados[acao].iloc[-1] / dados[acao].iloc[0] - 1
    performance_ativo = float(performance_ativo)

    if performance_ativo > 0:
        texto_performance_ativos = texto_performance_ativos + f"  \n{acao}: :green[{performance_ativo:.1%}]"
    elif performance_ativo < 0:
        texto_performance_ativos = texto_performance_ativos + f"  \n{acao}: :red[{performance_ativo:.1%}]"
    else:
        texto_performance_ativos = texto_performance_ativos + f"  \n{acao}: {performance_ativo:.1%}"
```

Finalmente, utilizamos a função **st.write()** para exibir os resultados na interface do Streamlit. O título dessa seção será menor do que o texto principal do aplicativo, o que podemos ajustar adicionando **###** antes do texto.

Como **texto\_performance\_ativos** é uma variável, utilizaremos uma f-string para que ela seja exibida corretamente.

```
st.write(f"""
### Performance dos Ativos
Essa foi a performance de cada ativo no período selecionado:

{texto_performance_ativos}
""")
```

Executando nosso aplicativo você verá que agora além do gráfico com os preços das ações temos um campo com as performances delas sendo exibidas dentro do período selecionado.

![performances das ações](https://www.hashtagtreinamentos.com/wp-content/uploads/2024/08/streamlit-aula-4-2-1024x581.png.webp)

performances das ações

### Performance de uma Carteira de Ações

Para concluir o projeto, vamos aprender a implementar o cálculo de performance para uma carteira de ações.

Para simplificar, vamos assumir pesos iguais para cada ativo. No entanto, você pode ajustar essa lógica para considerar diferentes pesos e valores para cada ativo.

Vamos começar criando uma carteira com um valor fixo de **R$1.000** alocado para cada ação presente na lista de ações. Inicialize a carteira logo abaixo do bloco onde verificamos quantas ações foram selecionadas.

Cada ação na variável carteira será inicializada com R$1.000, simulando um investimento inicial igual para cada ativo.

Em seguida, vamos calcular o valor total inicial da carteira somando os valores de todos os ativos.

```
carteira = [1000 for acao in lista_acoes]
total_inicial_carteira = sum(carteira)
```

Como a nossa carteira e a lista de ações estão organizadas na mesma ordem, podemos usar o **enumerate** para iterar sobre a lista\_acoes.

Dessa forma, conseguimos acessar tanto o nome da ação quanto o índice correspondente, permitindo calcular o valor de cada ticker na carteira de acordo com sua performance.

O valor de cada ativo é calculado multiplicando o valor inicial de R$1.000 pela performance do ativo. Por exemplo, se o ativo teve um crescimento de 10%, o novo valor será de R$1.100.

```
for i, acao in enumerate(lista_acoes):
    performance_ativo = dados[acao].iloc[-1] / dados[acao].iloc[0] - 1
    performance_ativo = float(performance_ativo)

    carteira[i] = carteira[i] * (1 + performance_ativo)
```

Agora, vamos calcular o valor total final da carteira, somando os valores atualizados de cada ativo. A performance total da carteira é obtida dividindo o valor final pelo valor inicial e subtraindo 1.

```
# calculo de perfomance
texto_performance_ativos = ""

if len(lista_acoes)==0:
    lista_acoes = list(dados.columns)
elif len(lista_acoes)==1:
    dados = dados.rename(columns={"Close": acao_unica})

carteira = [1000 for acao in lista_acoes]
total_inicial_carteira = sum(carteira)

for i, acao in enumerate(lista_acoes):
    performance_ativo = dados[acao].iloc[-1] / dados[acao].iloc[0] - 1
    performance_ativo = float(performance_ativo)

    carteira[i] = carteira[i] * (1 + performance_ativo)

    if performance_ativo > 0:
        # :cor[texto]
        texto_performance_ativos = texto_performance_ativos + f"  \n{acao}: :green[{performance_ativo:.1%}]"
    elif performance_ativo < 0:
        texto_performance_ativos = texto_performance_ativos + f"  \n{acao}: :red[{performance_ativo:.1%}]"
    else:
        texto_performance_ativos = texto_performance_ativos + f"  \n{acao}: {performance_ativo:.1%}"

total_final_carteira = sum(carteira)
performance_carteira = total_final_carteira / total_inicial_carteira - 1
```

Em seguida, formatamos o texto de exibição da performance total da carteira, seguindo a mesma lógica aplicada para a performance dos ativos.

```
# calculo de perfomance
texto_performance_ativos = ""

if len(lista_acoes)==0:
    lista_acoes = list(dados.columns)
elif len(lista_acoes)==1:
    dados = dados.rename(columns={"Close": acao_unica})

carteira = [1000 for acao in lista_acoes]
total_inicial_carteira = sum(carteira)

for i, acao in enumerate(lista_acoes):
    performance_ativo = dados[acao].iloc[-1] / dados[acao].iloc[0] - 1
    performance_ativo = float(performance_ativo)

    carteira[i] = carteira[i] * (1 + performance_ativo)

    if performance_ativo > 0:
        # :cor[texto]
        texto_performance_ativos = texto_performance_ativos + f"  \n{acao}: :green[{performance_ativo:.1%}]"
    elif performance_ativo < 0:
        texto_performance_ativos = texto_performance_ativos + f"  \n{acao}: :red[{performance_ativo:.1%}]"
    else:
        texto_performance_ativos = texto_performance_ativos + f"  \n{acao}: {performance_ativo:.1%}"

total_final_carteira = sum(carteira)
performance_carteira = total_final_carteira / total_inicial_carteira - 1

if performance_carteira > 0:
    texto_performance_carteira = f"Performance da carteira com todos os ativos: :green[{performance_carteira:.1%}]"
elif performance_carteira < 0:
    texto_performance_carteira = f"Performance da carteira com todos os ativos: :red[{performance_carteira:.1%}]"
else:
    texto_performance_carteira = f"Performance da carteira com todos os ativos: {performance_carteira:.1%}"

st.write(f"""
### Performance dos Ativos
Essa foi a perfomance de cada ativo no período selecionado:

{texto_performance_ativos}

{texto_performance_carteira}
""")
```

Finalmente, adicionamos **texto\_performance\_carteira** para ser exibido dentro da função **st.write()** junto com a performance de cada ativo.

![Performance de uma Carteira de Ações](https://www.hashtagtreinamentos.com/wp-content/uploads/2024/08/streamlit-aula-4-3-1024x598.png)

Performance de uma Carteira de Ações

Isso permitirá que o usuário visualize claramente o desempenho de seus investimentos ao longo do período selecionado.

### Deploy do Aplicativo

Com as implementações e edições desta aula, concluímos o projeto do aplicativo para verificação de preços de ações, que inclui um gráfico interativo, filtros e a performance dos ativos e das carteiras de ações.

No entanto, esse aplicativo só está disponível no seu computador. Para torná-lo acessível a outros usuários, é necessário fazer o **deploy**.

Você pode fazer o deploy através do botão de deploy dentro do Streamlit, o que publicará o aplicativo online, tornando-o acessível para toda a comunidade do Streamlit.

Caso prefira, siga o passo a passo que ensino nesta aula: [Deploy com o Streamlit – Como colocar seu site no Ar](https://www.hashtagtreinamentos.com/como-criar-sites-usando-python#aula2).

*[Voltar ao índice](https://www.hashtagtreinamentos.com/#rank-math-toc)*

## Curso de Streamlit – Aula 5 – Criando um Chat Interativo com Streamlit

Na aula anterior, exploramos como criar um dashboard de finanças usando o Streamlit, uma biblioteca Python que facilita a criação de aplicativos web interativos. Hoje, vamos dar um passo adiante e aprender a criar um **chat interativo** usando o Streamlit.

Esse chat pode ser personalizado para diversas finalidades, como responder perguntas, interagir com documentos ou até mesmo conectar-se a uma inteligência artificial.

Caso prefira esse conteúdo no formato de vídeo-aula, assista ao vídeo abaixo ou acesse o nosso [canal do YouTube](https://youtube.com/hashtagprograma%C3%A7%C3%A3o)!

****Para receber por e-mail o(s) arquivo(s) utilizados na aula, preencha**:**

Não vamos te encaminhar nenhum tipo de SPAM! A Hashtag Treinamentos é uma empresa preocupada com a proteção de seus dados e realiza o tratamento de acordo com a Lei Geral de Proteção de Dados (Lei n. 13.709/18). Qualquer dúvida, nos contate.

### O que você vai aprender?

Nesta aula, você vai aprender a:

1. **Criar uma interface de chat** usando os componentes do Streamlit.
2. **Armazenar e exibir mensagens** de forma dinâmica.
3. **Personalizar respostas** do chat, seja com respostas automáticas ou integração com uma IA.
4. **Melhorar a experiência do usuário** com elementos visuais e interativos.

Vamos começar do zero, partindo de um arquivo em branco e construindo o chat passo a passo.

### 1\. Instalando o Streamlit

Antes de começar, certifique-se de que o Streamlit está instalado no seu ambiente. Caso ainda não tenha instalado nas aulas anteriores, execute o seguinte comando no terminal:

```
pip install streamlit
```

### 2\. Estrutura Básica do Chat

Vamos começar criando a estrutura básica do nosso chat.

O Streamlit oferece componentes específicos para criar interfaces de chat, como `st.chat_input`  para capturar a mensagem do usuário e  `st.chat_message` para exibir as mensagens.

#### Importando as Bibliotecas

Primeiro, importamos as bibliotecas necessárias:

```
import streamlit as st
```

#### Criando o Título e Subtítulo

Vamos adicionar um título e um subtítulo ao nosso chat:

```
st.title("Meu Chat Interativo")
st.markdown("### Escreva e interaja com o nosso chat ao vivo!")
```

O título e o subtítulo ajudam a orientar o usuário sobre o que ele pode fazer no chat.

### 3\. Capturando a Mensagem do Usuário

Para capturar a mensagem do usuário, usamos o componente `st.chat_input`. Esse componente cria uma barra de entrada de texto com um botão de enviar.

```
mensagem_usuario = st.chat_input("Digite aqui sua mensagem")
```

A variável `mensagem_usuario`  armazenará o texto que o usuário digitar. Se o usuário não enviar nada, a variável será  `None`.

### 4\. Armazenando as Mensagens

Para manter um histórico das mensagens trocadas, usamos o **estado de sessão**  do Streamlit (`st.session_state`). O estado de sessão permite armazenar dados que persistem durante a interação do usuário com o aplicativo.

#### Inicializando o Histórico de Mensagens

Primeiro, verificamos se já existe um histórico de mensagens no estado de sessão. Caso não exista, inicializamos uma lista vazia.

```
if "mensagens" not in st.session_state:
    st.session_state.mensagens = []
```

#### Adicionando a Mensagem do Usuário ao Histórico

Se o usuário enviar uma mensagem, adicionamos essa mensagem ao histórico:

```
if mensagem_usuario:
    st.session_state.mensagens.append({"usuario": "user", "texto": mensagem_usuario})
```

Aqui, armazenamos a mensagem como um dicionário, contendo o tipo de usuário (`user`) e o texto da mensagem.

### 5\. Exibindo as Mensagens na Tela

Para exibir as mensagens na tela, usamos o componente `st.chat_message`. Esse componente permite exibir mensagens de diferentes tipos de usuários (como  `user`, `assistant`, `ai`, etc.) com estilos visuais distintos.

#### Percorrendo o Histórico de Mensagens

Vamos percorrer todas as mensagens armazenadas no estado de sessão e exibi-las na tela:

```
for mensagem in st.session_state.mensagens:
    with st.chat_message(mensagem["usuario"]):
        st.write(mensagem["texto"])
```

Esse código exibe cada mensagem com o estilo correspondente ao tipo de usuário.

### 6\. Adicionando Respostas Automáticas

Para tornar o chat mais interativo, vamos adicionar respostas automáticas. Neste exemplo, o chat responderá com uma mensagem fixa (“Ok”) sempre que o usuário enviar uma mensagem.

#### Adicionando a Resposta do Assistente

Após o usuário enviar uma mensagem, adicionamos uma resposta automática ao histórico:

```
if mensagem_usuario:
    st.session_state.mensagens.append({"usuario": "assistant", "texto": "Ok"})
```

Agora, o chat exibirá a mensagem do usuário e a resposta do assistente.

### 7\. Personalizando as Respostas

Para tornar o chat mais dinâmico, podemos personalizar as respostas do assistente. Por exemplo, podemos usar a biblioteca `random` para escolher uma resposta aleatória de uma lista.

#### Importando a Biblioteca Random

```
import random
```

#### Criando uma Lista de Respostas

```
respostas = ["Ok", "Tudo bem?", "Como posso ajudar?", "Interessante!"]
```

#### Escolhendo uma Resposta Aleatória

```
if mensagem_usuario:
    resposta = random.choice(respostas)
    st.session_state.mensagens.append({"usuario": "assistant", "texto": resposta})
```

Agora, o chat responderá com uma mensagem aleatória da lista.

### 8\. Executando o Aplicativo

Para executar o aplicativo, salve o código em um arquivo (por exemplo, `main.py`) e execute o seguinte comando no terminal:

```
streamlit run main.py
```

O aplicativo será aberto no navegador, e você poderá interagir com o chat.

### 9\. Próximos Passos

Este é apenas o começo! Aqui estão algumas ideias para expandir o chat:

- **Integração com IA**: Conecte o chat a uma API de inteligência artificial, como o ChatGPT, para respostas mais inteligentes.
- **Armazenamento em Banco de Dados**: Salve o histórico de mensagens em um banco de dados para persistência entre sessões.
- **Personalização Visual**: Adicione temas, ícones e estilos personalizados ao chat.

### Resumo

Nesta última parte, aprendemos a criar um chat interativo usando o Streamlit. Exploramos como capturar mensagens do usuário, armazenar o histórico de conversas e exibir as mensagens na tela. Além disso, vimos como personalizar as respostas do chat para torná-lo mais dinâmico.

## Conclusão – Curso de Streamlit – Como Criar Apps e Sites com Streamlit

Ao longo deste curso de Streamlit, você aprendeu a criar dois tipos de aplicativos: um **dashboard financeiro**  para monitorar preços de ações e um  **chat interativo**!

No primeiro projeto, exploramos como usar o Streamlit para importar dados do Yahoo Finance, manipular informações com Pandas e criar gráficos dinâmicos para visualizar a evolução dos preços das ações. Além disso, vimos como otimizar o desempenho do aplicativo com o uso de cache, garantindo uma experiência mais ágil para o usuário.

No segundo projeto, você aprendeu a construir um chat interativo, utilizando componentes específicos do Streamlit, como `st.chat_input`  e  `st.chat_message`. Aprendemos a capturar mensagens do usuário, armazenar o histórico de conversas com  `st.session_state` e exibir as mensagens de forma dinâmica na tela. Também implementamos respostas automáticas, personalizando as interações do chat para torná-lo mais dinâmico e útil.

Com as técnicas apresentadas, você agora tem as ferramentas necessárias para desenvolver seus próprios aplicativos web com Streamlit, seja para análise de dados, monitoramento financeiro ou criação de interfaces de chat interativas.

## Hashtag Treinamentos

Para acessar outras publicações de Python, !

---

Quer aprender mais sobre Python com um minicurso básico gratuito?

---

**Posts mais recentes de Python**

- 	Aprenda tudo sobre sets em Python: o que são, como usar e quando aplicar. Descubra operações com conjuntos e vantagens sobre listas!
- 	Aprenda como construir pipelines ETL com Python do zero! Descubra as melhores bibliotecas, resolva desafios comuns e torne-se um especialista!
- 	Aprenda a usar a biblioteca Pickle para salvar qualquer objeto Python em um arquivo e reutilizá-lo em outro código, sem precisar recriar tudo do zero.

**Posts mais recentes da Hashtag Treinamentos**

- 	Descubra o que é Power Apps, como a ferramenta revoluciona a criação de aplicativos e como você pode começar a usá-la hoje mesmo!
- 	Entenda os princípios básicos da linguagem R, sua importância e como começar a estudar de forma estruturada.
- 	Você sabe usar o Diagrama de Pareto? Nesta publicação vamos te ensinar tudo sobre o Gráfico de pareto! Quais as suas utilidades, como funciona e como criar!

Expert em conteúdos da Hashtag Treinamentos. Auxilia na criação de conteúdos de variados temas voltados para aqueles que acompanham nossos canais.