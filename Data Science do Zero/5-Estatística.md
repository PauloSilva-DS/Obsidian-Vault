---
tags:
  - dataScienceZero
  - estudo
  - python
Completo: false
Atualizado: 2025-02-19  17.00
Criado: 2025-02-19  16.03
---
[[0 -Data Science do Zero]]


Estatística é o campo da matemática que nos ajuda a entender e interpretar dados. Este capítulo fornece uma introdução básica, destacando conceitos essenciais para análise de conjuntos de dados.

#### **Descrevendo um Conjunto de Dados**

Para entender um conjunto de dados, podemos começar organizando as informações, como a contagem de amigos de usuários em uma rede social. Se o conjunto for pequeno, os números individuais podem ser suficientes, mas, para grandes volumes de dados, são necessárias estatísticas para resumir as informações de forma compreensível.

Uma maneira comum de visualizar a distribuição dos dados é através de um **histograma**, que mostra quantas vezes cada valor aparece. Além disso, algumas estatísticas básicas ajudam a descrever o conjunto:

- **Número de elementos**: Quantidade total de dados no conjunto.
- **Valor mínimo e máximo**: Os extremos do conjunto de dados.
- **Valores ordenados**: Permitem identificar os menores e maiores valores de forma mais precisa.

#### **Medidas de Tendência Central**

As medidas de tendência central ajudam a entender onde os dados estão concentrados. As principais são:

- **Média (Mean)**: Soma de todos os valores dividida pelo número total de elementos.
- **Mediana (Median)**: O valor central do conjunto quando os dados estão ordenados.
- **Moda (Mode)**: O valor mais frequente no conjunto.

A média é fácil de calcular, mas é sensível a valores extremos (outliers). A mediana, por outro lado, não é afetada por outliers e pode ser mais representativa em alguns casos.

Além disso, a **quantil** é uma generalização da mediana que divide o conjunto de dados em percentis, como os 10%, 25%, 50% (mediana), 75% e 90%.

Em geral, a estatística nos permite resumir e interpretar grandes conjuntos de dados de maneira eficaz, sendo essencial para análises e tomada de decisões.

### Dispersão

A **dispersão** mede o quão espalhados estão os dados em relação à sua tendência central. Estatísticas de dispersão são úteis para entender se os dados estão concentrados em torno de um valor central ou se variam amplamente. Alguns exemplos incluem:

- **Alcance (Range)**: A diferença entre o maior e o menor valor no conjunto de dados. Se o valor mínimo e o máximo forem iguais, a dispersão é zero; se forem distantes, a dispersão é grande.
    
    Exemplo de cálculo:
    
    ```python
    def data_range(xs: List[float]) -> float:
        return max(xs) - min(xs)
    ```
    
- **Variância (Variance)**: Mede a média dos desvios quadrados em relação à média. A variância é útil, mas pode ser difícil de interpretar, pois usa unidades quadradas dos dados (por exemplo, "amigos ao quadrado"). A fórmula usa n−1n-1 em vez de nn para corrigir a subestimação da variação quando se trabalha com uma amostra.
    
    Exemplo de cálculo:
    
    ```python
    def variance(xs: List[float]) -> float:
        deviations = de_mean(xs)
        return sum_of_squares(deviations) / (len(xs) - 1)
    ```
    
- **Desvio Padrão (Standard Deviation)**: A raiz quadrada da variância, que traz a medida de volta para as mesmas unidades dos dados originais. É uma das medidas mais usadas para entender a dispersão.
    
    Exemplo de cálculo:
    
    ```python
    def standard_deviation(xs: List[float]) -> float:
        return math.sqrt(variance(xs))
    ```
    
- **Faixa Interquartílica (Interquartile Range)**: A diferença entre o percentil 75% e o percentil 25%, que não é afetada por outliers, proporcionando uma medida de dispersão robusta.
    
    Exemplo de cálculo:
    
    ```python
    def interquartile_range(xs: List[float]) -> float:
        return quantile(xs, 0.75) - quantile(xs, 0.25)
    ```
    

### Correlação

A **correlação** mede como duas variáveis variam juntas. A **covariância** é uma medida básica, mas a **correlação** é mais útil porque normaliza os dados pela dispersão das duas variáveis, tornando a interpretação mais fácil.

- **Covariância**: Mede como duas variáveis se desviam em conjunto de suas médias. A fórmula calcula a soma dos produtos dos desvios de cada variável. A covariância pode ser difícil de interpretar, pois as unidades dependem das variáveis envolvidas.
    
    Exemplo de cálculo:
    
    ```python
    def covariance(xs: List[float], ys: List[float]) -> float:
        return dot(de_mean(xs), de_mean(ys)) / (len(xs) - 1)
    ```
    
- **Correlação**: Normaliza a covariância dividindo pelo desvio padrão de cada variável. O valor da correlação varia entre -1 (correlação negativa perfeita) e 1 (correlação positiva perfeita). Valores próximos de 0 indicam que não há relação linear significativa.
    
    Exemplo de cálculo:
    
    ```python
    def correlation(xs: List[float], ys: List[float]) -> float:
        stdev_x = standard_deviation(xs)
        stdev_y = standard_deviation(ys)
        return covariance(xs, ys) / stdev_x / stdev_y
    ```
    

#### Exemplo de Correlação com Outliers:

Uma análise de correlação pode ser sensível a **outliers**. No caso de um usuário com 100 amigos gastando apenas 1 minuto por dia, a correlação pode ser distorcida. Ao remover esse outlier, a correlação entre o número de amigos e o tempo gasto na plataforma aumenta consideravelmente.

Conclusão: A dispersão e a correlação são ferramentas poderosas para entender a variabilidade e as relações entre os dados, mas é importante estar atento aos outliers, que podem afetar significativamente as análises.

### Paradoxo de Simpson

O **Paradoxo de Simpson** é um fenômeno que ocorre quando uma correlação aparente entre duas variáveis desaparece (ou até se inverte) ao considerar uma variável de confusão. Em outras palavras, a correlação observada entre as variáveis principais pode ser enganosa se uma terceira variável, que não foi considerada inicialmente, influencia a relação.

#### Exemplo do Paradoxo de Simpson

Imaginemos que você está analisando quantos amigos os cientistas de dados da Costa Oeste e da Costa Leste têm. Inicialmente, você observa o seguinte:

|**Costa**|**Número de Membros**|**Média de Amigos**|
|---|---|---|
|Costa Oeste|10|8.2|
|Costa Leste|10|6.5|

A primeira impressão é que os cientistas de dados da Costa Oeste são mais amigáveis. No entanto, quando você analisa a educação dos membros, a correlação muda. Ao dividir os dados entre pessoas com doutorado e sem doutorado, o cenário se inverte:

|**Costa**|**Grau**|**Número de Membros**|**Média de Amigos**|
|---|---|---|---|
|Costa Oeste|PhD|3|3.1|
|Costa Leste|PhD|7|3.2|
|Costa Oeste|Sem PhD|6|10.9|
|Costa Leste|Sem PhD|3|13.4|

Quando você segmenta os dados por grau de educação, a média de amigos na Costa Leste para ambos os grupos (com ou sem PhD) é maior do que na Costa Oeste. O que inicialmente parecia uma diferença, na verdade, é causada pela distribuição desigual de PhDs entre as duas costas: a Costa Leste tem mais cientistas com PhD, que têm menos amigos em média.

#### O Paradoxo na Prática

Esse tipo de paradoxo é bastante comum no mundo real, especialmente quando há uma variável de confusão (como o grau acadêmico no exemplo acima) que não foi considerada na análise inicial. A correlação pode ser fortemente influenciada por como as variáveis são agrupadas ou segmentadas.

A chave para evitar o Paradoxo de Simpson é conhecer bem os dados e tentar identificar e controlar possíveis variáveis de confusão. Às vezes, pode ser impossível controlar completamente todas as variáveis, especialmente se você não tiver acesso a informações adicionais.

#### Outras Armadilhas da Correlação

- **Correlação Zero**: Uma correlação de zero entre duas variáveis indica que **não há** uma relação linear entre elas. No entanto, pode haver outros tipos de relação não linear. Por exemplo, no caso de x=[−2,−1,0,1,2]x = [-2, -1, 0, 1, 2] e y=[2,1,0,1,2]y = [2, 1, 0, 1, 2], a correlação entre xx e yy é zero, mas yy é a **valor absoluto** de xx, o que configura uma relação, mas não linear.
    
- **Tamanho da Relação**: A correlação indica o quão linearmente relacionadas estão duas variáveis, mas não diz nada sobre a **magnitude** da relação. Por exemplo, x=[−2,−1,0,1,2]x = [-2, -1, 0, 1, 2] e y=[99.98,99.99,100,100.01,100.02]y = [99.98, 99.99, 100, 100.01, 100.02] têm uma correlação perfeita, mas essa relação pode não ser significativa dependendo do contexto e das unidades de medida.
    

### Conclusão

O **Paradoxo de Simpson** nos lembra que a correlação pode ser enganosa se não controlarmos adequadamente as variáveis de confusão. Além disso, uma correlação de zero ou uma correlação perfeita nem sempre é útil ou significativa sem entender o contexto mais amplo dos dados. Ao realizar uma análise, é essencial questionar a origem e as possíveis causas das correlações observadas, principalmente quando há subgrupos ou fatores adicionais envolvidos.

### Correlação e Causalidade

A expressão “correlação não implica causalidade” é amplamente conhecida, especialmente em contextos em que os dados revelam algo inesperado ou desafiador. Esta é uma consideração crucial quando estamos analisando dados, porque, embora a correlação entre duas variáveis possa sugerir uma relação, ela não nos diz nada sobre **qual** variável causa a outra.

#### Exemplos de Possíveis Relações Causais

1. **Mais amigos causam mais tempo no site**: Pode ser que, ao ter mais amigos no site, o usuário precise passar mais tempo lá para se manter atualizado com as postagens de seus amigos. Nesse caso, a correlação entre **num_friends** (número de amigos) e **daily_minutes** (tempo diário no site) pode ser explicada pela causalidade de que mais amigos **provocam** mais tempo gasto.
    
2. **Mais tempo no site causa mais amigos**: Outra explicação possível é que os usuários que passam mais tempo no site, especialmente interagindo em fóruns ou publicações, acabam encontrando e fazendo mais amigos. Aqui, o tempo gasto no site pode ser a causa do aumento no número de amigos.
    
3. **Fatores externos e a correlação mútua**: Uma terceira explicação possível é que os usuários mais apaixonados por ciência de dados, por exemplo, são tanto mais ativos no site quanto mais tendem a coletar amigos que compartilham seu interesse. Nesse caso, tanto o tempo gasto quanto o número de amigos são causados por uma paixão comum pela área.
    

#### Como Determinar a Causalidade?

Uma maneira eficaz de investigar a causalidade é por meio de **experimentos randomizados**. Ao dividir aleatoriamente um grupo de usuários em dois grupos com características demográficas semelhantes e oferecer experiências ligeiramente diferentes a cada grupo, podemos começar a entender se uma variável realmente causa a outra.

Por exemplo, imagine que você quer testar se **ter mais amigos** realmente **causa** mais tempo no site. Você poderia, aleatoriamente, mostrar a um grupo de usuários apenas o conteúdo de uma fração de seus amigos e ver se eles passam menos tempo no site. Se isso acontecer, você teria mais confiança na ideia de que **ter mais amigos causa mais tempo gasto no site**.

#### Reflexões Importantes

- **Causalidade não é simples**: Mesmo experimentos podem ser afetados por variáveis não observadas ou problemas no design do experimento.
- **Correlação pode ser enganosa**: Como vimos no Paradoxo de Simpson, a correlação pode ser confundida por outras variáveis ou pela maneira como os dados são agrupados.

#### Recursos para Exploração Adicional

Para quem deseja se aprofundar mais em estatísticas e entender melhor os conceitos de correlação, causalidade, e outros, existem várias referências úteis, incluindo:

- **Introductory Statistics** por Douglas Shafer e Zhiyi Zhang (Saylor Foundation)
- **OnlineStatBook** por David Lane (Rice University)
- **Introductory Statistics** por OpenStax (OpenStax College)

Além disso, bibliotecas como **SciPy**, **pandas** e **StatsModels** oferecem funções estatísticas avançadas para análise de dados em Python. Se você deseja aprimorar suas habilidades como cientista de dados, estudar estatísticas é essencial.

### Conclusão

A correlação é uma ferramenta poderosa para descobrir padrões, mas é preciso ter cuidado ao tirar conclusões precipitadas sobre causalidade. Experimentos bem desenhados e uma compreensão profunda dos dados são fundamentais para uma análise robusta e para evitar enganos na interpretação de dados.