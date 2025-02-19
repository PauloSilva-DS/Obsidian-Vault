---
tags:
  - dataScienceZero
  - estudo
  - python
Completo: false
Atualizado: 2025-02-19  16.13
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

