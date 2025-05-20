---
title: "Streamlit: guia completo para criar web apps interativos rapidamente"
source: "https://hub.asimov.academy/blog/streamlit-guia-completo/"
author:
  - "[[Ana Maria Gomes]]"
published: 2024-09-27
created: 2025-05-20
description: "Tudo o que você precisa saber sobre Streamlit e como criar web apps interativos rapidamente, sem conhecimentos avançados de desenvolvimento."
tags:
  - "clippings"
---
Quer criar web apps interativos com apenas algumas linhas de código e sem precisar usar HTML, CSS ou JavaScript? Então o **Streamlit** é a ferramenta que você precisa conhecer!

Neste guia, você vai aprender como essa [biblioteca de Python](https://hub.asimov.academy/blog/bibliotecas-python/) pode transformar sua experiência com dados em web apps de forma prática e rápida! Te mostraremos desde a configuração do ambiente até a criação de web apps interativos e dinâmicos.

Caso prefira uma abordagem no formato videoaula, conheça o [Python para iniciantes: do zero ao primeiro projeto](https://hub.asimov.academy/curso/python-para-iniciantes-do-zero-ao-primeiro-projeto/), o curso gratuito da Asimov que te ensina a criar um web app com Streamlit em tempo recorde!

Curso Gratuito

![](https://hub.asimov.academy/wp-content/uploads/2024/08/ThumbPython_Para_Iniciantes_MASTERCLASS.webp)

## Seu primeiro projeto Python – curso grátis com certificado!

Vá do zero ao primeiro projeto em apenas 2 horas com o curso Python para Iniciantes.

[Comece agora](https://asimov.academy/curso-gratuito-python/?utm_source=blog&utm_medium=organic&utm_campaign=internal_link&utm_content=Streamlit%3A+guia+completo+para+criar+web+apps+interativos+rapidamente "Comece agora")

## O que é Streamlit?

[Streamlit](https://streamlit.io/) é uma biblioteca open-source em Python que permite a criação de aplicativos web para análise de dados de forma extremamente rápida. Com ela, você pode transformar scripts de dados em web apps compartilháveis com poucas linhas de código. Ou seja, é uma ferramenta poderosa para cientistas de dados, analistas e desenvolvedores que desejam visualizar e interagir com seus dados de maneira dinâmica, sem a necessidade de conhecimento avançado em desenvolvimento web.

![Logo da biblioteca Streamlit de Python](https://hub.asimov.academy/wp-content/uploads/2024/09/Streamlit-logo-1024x599.png)

Logo da biblioteca Streamlit de Python

### Para que serve o Streamlit?

O Streamlit se destaca por simplificar o desenvolvimento de interfaces interativas e dashboards, sendo ideal para prototipagem rápida e compartilhamento de dados. Entre suas principais vantagens estão:

- Criação de interfaces interativas com widgets;
- Integração fácil com bibliotecas como Matplotlib, Seaborn e Plotly para visualização de dados;
- Facilidade para hospedar e compartilhar aplicativos na web;
- Ideal para machine learning, permitindo testar modelos, ajustar hiperparâmetros e visualizar previsões em tempo real;
- Inclusão de widgets como sliders, checkboxes e seletores, facilitando a interação do usuário com dados e simulações.

Essas características tornam o Streamlit uma excelente escolha para quem precisa apresentar dados de forma dinâmica e acessível, mesmo para equipes com pouca experiência em programação.

No entanto, Streamlit vai bem além de dashboards e web apps para dados, como explica o no vídeo abaixo:

![](https://www.youtube.com/watch?v=8FJRQQolqNc)

### Por que usar Streamlit para criar web apps?

Uma das grandes vantagens do Streamlit é a simplicidade. Para quem já trabalha com [Python](https://hub.asimov.academy/blog/python-o-que-e/), a curva de aprendizado é bastante suave, pois a criação de interfaces se dá de maneira declarativa, muito semelhante à construção de códigos comuns. Basta instalar a biblioteca e, com poucos comandos, é possível transformar um código Python em uma aplicação web interativa, sem lidar com JavaScript, HTML ou CSS.

Outro ponto forte é a sua agilidade no desenvolvimento. Com apenas algumas linhas de código, você consegue visualizar dados, criar gráficos interativos e adicionar controles como sliders, caixas de seleção e botões, o que agiliza a prototipagem e o desenvolvimento de ferramentas internas. Além disso, o Streamlit permite atualizações automáticas: sempre que o código é alterado, a aplicação é recarregada instantaneamente.

É toda essa agilidade e facilidade que mostramos no vídeo a seguir, onde aceitamos um desafio que recebemos no [Instagram](https://www.instagram.com/asimov.academy/) : construir um dashboard com Python em **menos** **de 30 minutos**.

![](https://www.youtube.com/watch?v=P6E_Kts9pxE)

Agora, veja o passo a passo para iniciar seus trabalhos com esta biblioteca super-rápida!

## Como configurar o ambiente do Streamlit?

Antes de iniciar o desenvolvimento de um projeto com Streamlit, é necessário configurar o ambiente. Vamos ver como fazer isso.

### Install Streamlit: instalação da biblioteca

A instalação do Streamlit será feita com pip, o gerenciador de pacotes do Python. Dessa forma, abra o terminal e digite este comando:

## Como criar um web app com Streamlit?

Com o ambiente configurado, vamos criar um web app com uma página bem simples. Para isso, criaremos um arquivo [Python](https://www.python.org/) chamado app.py na pasta do projeto. Nesse arquivo, vamos codar todos os comandos que vêm a seguir.

### Estrutura básica de um app Streamlit

A estrutura básica de um script Streamlit é bastante simples. Você só precisa importar a biblioteca e começar a adicionar elementos ao seu app.

Escrevemos título na página e um texto simples, apenas declarando os comandos `st.title()` e `st.write()` no script da aplicação. Isso já evidencia a facilidade em usar o framework no desenvolvimento de web apps.

Para executar o aplicativo, volte ao terminal e digite:

Isso abrirá uma nova janela do navegador com seu primeiro aplicativo Streamlit em funcionamento.

![](https://hub.asimov.academy/wp-content/uploads/2024/09/primeiroAPP-1024x365.webp)

## Formas de exibir tabelas no Streamlit

O Streamlit oferece várias maneiras de exibir [tabelas](https://hub.asimov.academy/tutorial/trabalhando-com-tabelas-no-streamlit-um-guia-para-iniciantes/) de dados. Vamos explorar algumas delas.

### 1\. Função st.write

A função `st.write` é uma das mais versáteis do Streamlit. Você pode usá-la para exibir texto, tabelas, gráficos e muito mais.

![tabela usando st.write no streamlit](https://hub.asimov.academy/wp-content/uploads/2024/09/primeiratabela-1024x342-1.webp)

tabela usando st.write no streamlit

### 2\. Função st.dataframe

A função `st.dataframe` permite exibir uma tabela interativa, onde o usuário pode ordenar e filtrar os dados apenas clicando no cabeçalho das colunas.

```
st.dataframe(df)
```

### 3\. Função st.table

A função `st.table` exibe uma tabela estática, ideal para quando você não precisa de interatividade.

```
st.table(df)
```

![Tabelas no streamlit usando st.dataframe e st.table](https://hub.asimov.academy/wp-content/uploads/2024/09/dftable-1024x502-1.webp)

Tabelas no streamlit usando st.dataframe e st.table

## Tipos de gráficos em Streamlit

O Streamlit permite criar diferentes tipos de [gráficos](https://hub.asimov.academy/tutorial/exibindo-graficos-no-streamlit-um-guia-para-iniciantes/) de forma simples, pois ele já oferece métodos específicos para cada tipo.

### 1\. Gráficos de linha

Os gráficos de linha são ideais para mostrar tendências ao longo do tempo.

![gráfico de linha no streamlit](https://hub.asimov.academy/wp-content/uploads/2024/09/linechart-1024x512-1.webp)

gráfico de linha no streamlit

### 2\. Gráficos de barra

Os gráficos de barra são bons para comparar diferentes categorias.

```
st.bar_chart(data)
```

![Gráfico de barra no streamlit](https://hub.asimov.academy/wp-content/uploads/2024/09/barchart-1024x465-1.webp)

Gráfico de barra no streamlit

### 3\. Gráficos de área

Os gráficos de área são úteis para mostrar a evolução de uma variável ao longo do tempo.

```
st.area_chart(data)
```

![Gráfico de área no streamlit](https://hub.asimov.academy/wp-content/uploads/2024/09/areachart-1024x487-1.webp)

Gráfico de área no streamlit

## Exibição de mapas com Streamlit

Além da exibição de gráficos, o Streamlit oferece facilidade para [exibição de mapas](https://hub.asimov.academy/tutorial/exibindo-mapas-com-streamlit-de-maneira-simples/), com a função `st.map`. Essa função permite, de maneira simples e rápida, renderizar mapas interativos.

![Mapa no streamlit](https://hub.asimov.academy/wp-content/uploads/2024/09/mapa-1024x614-1.webp)

Mapa no streamlit

## Interatividade com widgets

[Adicionar widgets ao seu web app](https://hub.asimov.academy/tutorial/widgets-de-streamlit-sliders-botoes-caixas-de-selecao-e-mais/) com Streamlit é uma maneira de aumentar a interatividade e melhorar a experiência do usuário. Esses componentes interativos permitem que os usuários interajam diretamente com a aplicação, oferecendo opções de entrada e controle. Além de tornar o app mais dinâmico, widgets são fundamentais para criar interfaces responsivas e adaptadas às necessidades do usuário.

Vejamos como adicionar alguns widgets ao nosso app.

### Sliders

Os sliders são widgets que permitem aos usuários selecionar um valor dentro de um intervalo.

![Exemplo de sliders como widget](https://hub.asimov.academy/wp-content/uploads/2024/09/sliders-1024x328-1.webp)

Exemplo de sliders como widget

### Botões

Os botões são widgets que permitem aos usuários acionar ações específicas.

![Exemplo de botão como widget](https://hub.asimov.academy/wp-content/uploads/2024/09/button-1024x411-1.webp)

Exemplo de botão como widget

### Caixas de seleção

As caixas de seleção são widgets que permitem aos usuários selecionar uma ou mais opções de uma lista.

![Exemplo do widget caixa de seleção](https://hub.asimov.academy/wp-content/uploads/2024/09/caixadeselecao-1024x374-1.webp)

Exemplo do widget caixa de seleção

## Entradas de texto com Streamlit

As [entradas de texto](https://hub.asimov.academy/tutorial/entradas-de-texto-com-streamlit/) são componentes fundamentais em qualquer aplicação web. Elas permitem que os usuários insiram dados que podem ser processados e utilizados pelo aplicativo.

### Como criar uma entrada de texto simples com st.text\_input?

Criar uma entrada de texto no Streamlit é muito simples. No exemplo, o usuário informa seu nome, dá enter e logo abaixo é exibida a mensagem “Olá, nome\_usuario”.

![Exemplo de entrada de texto no streamlit](https://hub.asimov.academy/wp-content/uploads/2024/09/entrada1-1024x347-1.webp)

Exemplo de entrada de texto no streamlit

### Personalização da entrada de texto

Você pode personalizar a entrada de texto de várias maneiras. Por exemplo, adicionando um valor inicial, um placeholder e muito mais.

![Entrada de texto personalizada com streamlit](https://hub.asimov.academy/wp-content/uploads/2024/09/entrada2.webp)

Entrada de texto personalizada com streamlit

## Atualização em tempo real de web apps em Streamlit

A atualização em tempo real em web apps é crucial para fornecer informações imediatas, melhorando a experiência do usuário e a eficiência do aplicativo. Isso é útil em dashboards de monitoramento, dados financeiros e aplicativos que dependem de informações dinâmicas, onde os usuários precisam tomar decisões rápidas, com base em dados atualizados.

No Streamlit, essa atualização ocorre de maneira automática a cada interação com o app. Isso acontece através de um processo de “polling”, no qual o aplicativo constantemente verifica mudanças no estado ou nos dados, sem que o usuário precise recarregar a página. Sendo assim, cada vez que um componente interativo, como um botão ou um slider, é acionado, o código do app é reexecutado, garantindo que as informações exibidas estejam sempre atualizadas.

Acesse o tutorial abaixo e confira o exemplo de código de implementação desse tipo de atualização no Streamlit:### [Atualizando seu Web App em Tempo Real com Streamlit e Widgets](https://hub.asimov.academy/tutorial/atualizando-web-app-tempo-real-com-streamlit-widgets/ "Ir para Atualizando seu Web App em Tempo Real com Streamlit e Widgets")

## Gerenciamento de estado no Streamlit

O gerenciamento de estado em aplicativos web com Streamlit é essencial para manter a interatividade e garantir que informações sejam preservadas ao longo de toda a sessão do usuário. Sem essa funcionalidade, cada interação, como cliques e atualizações, poderia resultar na perda de dados, o que comprometeria a experiência do usuário. O gerenciamento de estado no Streamlit assegura que [variáveis](https://hub.asimov.academy/blog/variaveis-constantes-programacao/) e inputs dos usuários sejam mantidos, permitindo uma experiência contínua e fluida.

No Streamlit, o **Session State** desempenha o papel de armazenar dados temporários durante a execução do aplicativo. Ele permite que o app “lembre-se” de valores e interações anteriores, mesmo que o script seja reexecutado a cada interação. Isso é especialmente útil em formulários, cálculos ou outras configurações que precisam ser preservadas ao longo do uso, garantindo que o aplicativo funcione de maneira dinâmica e eficiente.

Aprenda a aplicar **Session State** no seu web app por meio deste tutorial:### [Entendendo o Gerenciamento de Estado no Streamlit](https://hub.asimov.academy/tutorial/entendendo-o-gerenciamento-de-estado-no-streamlit/ "Ir para Entendendo o Gerenciamento de Estado no Streamlit")

## Melhorando a performance com cache

O uso de [cache no Streamlit](https://hub.asimov.academy/tutorial/cache-com-streamlit-um-guia-simples-para-iniciantes/) pode trazer vários benefícios, como melhoria de desempenho, maior eficiência e melhor experiência do usuário. O caching permite que o aplicativo armazene resultados de funções ou dados que não mudam frequentemente, evitando a necessidade de recalcular ou recarregar informações repetidamente. Isso é essencial em aplicações que realizam operações pesadas ou que acessam dados de fontes externas, como APIs ou bancos de dados, onde o tempo de resposta pode ser um fator crítico. Com o cache, os usuários podem acessar rapidamente os dados que já foram processados, resultando em tempos de carregamento mais rápidos e uma interação mais suave com o aplicativo.

O Streamlit oferece uma maneira simples de implementar caching usando o decorador `@st.cache_data`, que armazena os resultados de uma função em memória. Quando a função é chamada novamente com os mesmos parâmetros, o Streamlit retorna os dados do cache em vez de executar a função novamente. Isso não apenas economiza tempo de computação, mas também reduz a carga em recursos externos, tornando a aplicação mais eficiente. Ao utilizar o cache adequadamente, você pode garantir que seu web app ofereça uma experiência de usuário mais responsiva e agradável, permitindo que os usuários se concentrem em suas tarefas sem se preocupar com atrasos desnecessários.

### Como usar cache no Streamlit?

O Streamlit oferece dois decoradores principais para cache: `@st.cache_data` e `@st.cache_resource`.

#### 1\. Decorador @st.cache\_data

#### 2\. Decorador @st.cache\_resource

## Aplicação prática: criação de aplicativo

Agora, vamos criar um aplicativo que permite ao usuário escolher um conjunto de dados para visualizar e interagir com gráficos de linhas, barras e mapas, além de armazenar as preferências do usuário usando Session State. É uma aplicação meramente ilustrativa, para visualizarmos uma página completa, feita apenas com Streamlit e com os elementos que estudamos neste material.

```
import streamlit as st

import pandas as pd

import numpy as np

import matplotlib.pyplot as plt

# Função para gerar dados aleatórios

@st.cache_data

def generate_data(num_points):

    return pd.DataFrame(

        np.random.randn(num_points, 3),

        columns=['A', 'B', 'C']

    )

# Inicializar o Session State

if 'num_points' not in st.session_state:

    st.session_state.num_points = 100

# Configurações do app

st.title("Exemplo Completo de Web App com Streamlit")

st.sidebar.header("Configurações")

# Slider para escolher o número de pontos

num_points = st.sidebar.slider("Escolha o número de pontos:", min_value=10, max_value=500, value=st.session_state.num_points)

st.session_state.num_points = num_points

# Gerar dados com base na escolha do usuário

data = generate_data(st.session_state.num_points)

# Exibir gráficos

st.header("Gráficos")

grafico_tipo = st.selectbox("Escolha o tipo de gráfico:", ["Linha", "Barra", "Área"])

if grafico_tipo == "Linha":

    st.line_chart(data)

elif grafico_tipo == "Barra":

    st.bar_chart(data)

elif grafico_tipo == "Área":

    st.area_chart(data)

# Exibir mapa com dados aleatórios

st.header("Mapa com Dados Aleatórios")

map_data = pd.DataFrame(

    np.random.randn(100, 2) / [50, 50] + [37.76, -122.4],

    columns=['lat', 'lon']

)

st.map(map_data)

# Caixas de seleção para mostrar informações adicionais

st.sidebar.header("Opções Adicionais")

show_stats = st.sidebar.checkbox("Mostrar estatísticas descritivas", value=True)

if show_stats:

    st.subheader("Estatísticas Descritivas dos Dados")

    st.write(data.describe())

# Botão para resetar as configurações

if st.sidebar.button("Resetar"):

    st.session_state.num_points = 100

    st.experimental_rerun()
```

Aqui o usuário pode escolher o número de pontos no gráfico com sliders e ainda selecionar o tipo do gráfico que deseja para exibição dos dados.

![Exemplo completo de web app com streamlit](https://hub.asimov.academy/wp-content/uploads/2024/09/app1-1024x516-1.webp)

Exemplo completo de web app com streamlit

Descendo na página, temos a aplicação da exibição de mapa e, interagindo com a caixa de seleção, são organizados em tabela dados de estatística descritiva. Também utilizamos cache e session state.

![Exibição de mapa em web app](https://hub.asimov.academy/wp-content/uploads/2024/09/app2-1024x519-1.webp)

Exibição de mapa em web app

## Aplique as boas práticas de desenvolvimento

Para garantir que seu web app seja eficiente e fácil de usar, é importante seguir algumas boas práticas de desenvolvimento.

### Evite erros comuns

- Inicialize todas as variáveis necessárias.
- Verifique a existência de chaves antes de acessá-las no Session State.

### Aplique estas práticas ao usar Session State

- Use [callbacks](https://hub.asimov.academy/blog/callbacks-basicos/) para atualizar o estado.
- Mantenha o código organizado.

### Otimize a performance do seu web app

- Reduza o tamanho dos dados.
- Use gráficos eficientes.
- Evite loops desnecessários.

## Conclusão

O Streamlit é uma ferramenta que democratiza o desenvolvimento de web apps, tornando a criação de interfaces interativas acessível a todos. À medida que a demanda por soluções ágeis e personalizadas cresce, ferramentas como o Streamlit se tornam essenciais, permitindo que desenvolvedores e [profissionais de dados](https://hub.asimov.academy/blog/diferencas-engenheiro-de-dados-cientista-de-dados-analista-de-dados/) criem soluções robustas e dinâmicas sem complicações.

Com a combinação de simplicidade e poder, o Streamlit nos mostra que o futuro da tecnologia está na inovação, acessibilidade e compartilhamento de conhecimento, onde qualquer pessoa pode transformar ideias em aplicativos funcionais.

## Você também pode gostar:[BLOG](https://hub.asimov.academy/blog/deploy-guia-completo/ "Acessar o post Deploy: guia completo para publicar suas aplicações Python")

Deploy: guia completo para publicar suas aplicações Python

![O que é Deploy?](https://hub.asimov.academy/wp-content/uploads/2025/04/o-que-e-deploy.png)

![Avatar de Heitor Tasso](https://hub.asimov.academy/wp-content/uploads/2025/01/01-11-2021-SENAC.jpg) Heitor Tasso • 27 dias atrás

[View original](https://hub.asimov.academy/blog/deploy-guia-completo/) "Acessar o post Deploy: guia completo para publicar suas aplicações Python"

![Imagem de um notebook](https://hub.asimov.academy/wp-content/themes/hubasimov/assets/img/mockup-cta.png)

## Cursos de programação gratuitos com certificado

Aprenda a programar e desenvolva soluções para o seu trabalho com Python para alcançar novas oportunidades profissionais. Aqui na Asimov você encontra:

- Conteúdos gratuitos
- Projetos práticos
- Certificados
- +20 mil alunos e comunidade exclusiva
- Materiais didáticos e download de código
[Inicie agora](https://hub.asimov.academy/registrar "Ir para a página de cadastro")