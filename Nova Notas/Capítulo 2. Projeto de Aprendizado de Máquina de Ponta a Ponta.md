---
Atualizado: 2025-03-07  15.27
Criado: 2025-03-07  14.55
---
até aqui pagina 50 do 70 até 155

---

### Escalonamento e Transformação de Características

Uma das transformações mais importantes que você precisa aplicar aos seus dados é o **escalonamento de características**. Com poucas exceções, os algoritmos de aprendizado de máquina não performam bem quando os atributos numéricos de entrada têm escalas muito diferentes. Esse é o caso dos dados de habitação: o número total de quartos varia de cerca de 6 a 39.320, enquanto as rendas medianas variam apenas de 0 a 15. Sem nenhum escalonamento, a maioria dos modelos tenderá a ignorar a renda mediana e focar mais no número de quartos.

Existem duas maneiras comuns de fazer com que todos os atributos tenham a mesma escala: **escalonamento min-max** e **padronização**.

---

## AVISO

Como acontece com todos os estimadores, é importante ajustar os escalonadores apenas aos dados de treinamento: nunca use `fit()` ou `fit_transform()` para qualquer coisa que não seja o conjunto de treinamento. Uma vez que você tenha um escalonador treinado, pode usá-lo para `transform()` qualquer outro conjunto, incluindo o conjunto de validação, o conjunto de teste e novos dados. Observe que, embora os valores do conjunto de treinamento sempre sejam escalonados para o intervalo especificado, se novos dados contiverem outliers, eles podem acabar escalonados fora do intervalo. Se você quiser evitar isso, basta definir o hiperparâmetro `clip` como `True`.

---

O **escalonamento min-max** (muitas pessoas chamam isso de **normalização**) é o mais simples: para cada atributo, os valores são deslocados e redimensionados para que acabem variando de 0 a 1. Isso é feito subtraindo o valor mínimo e dividindo pela diferença entre o mínimo e o máximo. O Scikit-Learn fornece um transformador chamado `MinMaxScaler` para isso. Ele tem um hiperparâmetro `feature_range` que permite alterar o intervalo se, por algum motivo, você não quiser 0–1 (por exemplo, redes neurais funcionam melhor com entradas de média zero, então um intervalo de –1 a 1 é preferível). É bem fácil de usar:

```python
from sklearn.preprocessing import MinMaxScaler

min_max_scaler = MinMaxScaler(feature_range=(-1, 1))  
housing_num_min_max_scaled = min_max_scaler.fit_transform(housing_num)
```

---

A **padronização** é diferente: primeiro, ela subtrai o valor médio (de modo que os valores padronizados tenham média zero) e, em seguida, divide o resultado pelo desvio padrão (de modo que os valores padronizados tenham um desvio padrão igual a 1). Diferente do escalonamento min-max, a padronização não restringe os valores a um intervalo específico. No entanto, a padronização é muito menos afetada por outliers. Por exemplo, suponha que um distrito tenha uma renda mediana igual a 100 (por engano), em vez do usual 0–15. O escalonamento min-max para o intervalo 0–1 mapearia esse outlier para 1 e esmagaria todos os outros valores para 0–0.15, enquanto a padronização não seria muito afetada. O Scikit-Learn fornece um transformador chamado `StandardScaler` para padronização:

```python
from sklearn.preprocessing import StandardScaler

std_scaler = StandardScaler()
housing_num_std_scaled = std_scaler.fit_transform(housing_num)
```

---

### DICA

Se você quiser escalonar uma matriz esparsa sem convertê-la primeiro em uma matriz densa, pode usar um `StandardScaler` com o hiperparâmetro `with_mean` definido como `False`: ele apenas dividirá os dados pelo desvio padrão, sem subtrair a média (já que isso quebraria a esparsidade).

Quando a distribuição de uma característica tem uma **cauda pesada** (ou seja, quando valores distantes da média não são exponencialmente raros), tanto o escalonamento min-max quanto a padronização comprimirão a maioria dos valores em um intervalo pequeno. Modelos de aprendizado de máquina geralmente não gostam disso, como você verá no Capítulo 4. Portanto, antes de escalonar a característica, você deve primeiro transformá-la para reduzir a cauda pesada e, se possível, tornar a distribuição aproximadamente simétrica. Por exemplo, uma maneira comum de fazer isso para características positivas com uma cauda pesada à direita é substituir a característica por sua raiz quadrada (ou elevar a característica a uma potência entre 0 e 1). Se a característica tiver uma cauda realmente longa e pesada, como uma distribuição de lei de potência, substituir a característica por seu logaritmo pode ajudar. Por exemplo, a característica de população segue aproximadamente uma lei de potência:

---

Distritos com 10.000 habitantes são apenas 10 vezes menos frequentes do que distritos com 1.000 habitantes, e não exponencialmente menos frequentes. A Figura 2-17 mostra o quanto melhor essa característica fica quando você calcula seu logaritmo: ela fica muito próxima de uma distribuição Gaussiana (ou seja, em forma de sino).

---

Outra abordagem para lidar com características de cauda pesada consiste em **dividir em intervalos** (bucketizing) a característica. Isso significa cortar sua distribuição em intervalos de tamanho aproximadamente igual e substituir cada valor da característica pelo índice do intervalo ao qual ele pertence, de forma semelhante ao que fizemos para criar a característica `income_cat` (embora tenhamos usado isso apenas para amostragem estratificada). Por exemplo, você poderia substituir cada valor pelo seu percentil. A divisão em intervalos de tamanho igual resulta em uma característica com uma distribuição quase uniforme, então não há necessidade de escalonamento adicional, ou você pode simplesmente dividir pelo número de intervalos para forçar os valores ao intervalo 0–1.

Quando uma característica tem uma distribuição **multimodal** (ou seja, com dois ou mais picos claros, chamados modos), como a característica `housing_median_age`, também pode ser útil dividi-la em intervalos, mas desta vez tratando os IDs dos intervalos como categorias, em vez de valores numéricos. Isso significa que os índices dos intervalos devem ser codificados, por exemplo, usando um `OneHotEncoder` (então você geralmente não quer usar muitos intervalos). Essa abordagem permitirá que o modelo de regressão aprenda mais facilmente regras diferentes para diferentes faixas de valores dessa característica. Por exemplo, talvez casas construídas há cerca de 35 anos tenham um estilo peculiar que caiu em desuso e, portanto, sejam mais baratas do que sua idade sugere.

---

Outra abordagem para transformar distribuições multimodais é adicionar uma característica para cada um dos modos (pelo menos os principais), representando a similaridade entre a idade mediana da habitação e aquele modo específico. A medida de similaridade é tipicamente calculada usando uma **função de base radial** (RBF, do inglês *radial basis function*) — qualquer função que depende apenas da distância entre o valor de entrada e um ponto fixo. A RBF mais comumente usada é a RBF Gaussiana, cujo valor de saída decai exponencialmente à medida que o valor de entrada se afasta do ponto fixo. Por exemplo, a similaridade RBF Gaussiana entre a idade da habitação \( x \) e 35 é dada pela equação \(\exp(-y(x-35)^2)\). O hiperparâmetro \( y \) (gama) determina a rapidez com que a medida de similaridade decai à medida que \( x \) se afasta de 35. Usando a função `rbf_kernel()` do Scikit-Learn, você pode criar uma nova característica RBF Gaussiana medindo a similaridade entre a idade mediana da habitação e 35:

```python
from sklearn.metrics.pairwise import rbf_kernel

age_simil_35 = rbf_kernel(housing[["housing_median_age"]], [[35]], gamma=0.1)
```

---

A Figura 2-18 mostra essa nova característica como uma função da idade mediana da habitação (linha sólida). Ela também mostra como a característica ficaria se você usasse um valor menor para gama. Como o gráfico mostra, a nova característica de similaridade de idade atinge o pico em 35, exatamente ao redor do pico na distribuição da idade mediana da habitação: se esse grupo etário específico estiver bem correlacionado com preços mais baixos, há uma boa chance de que essa nova característica ajude.

---

Até agora, só olhamos para as características de entrada, mas os valores alvo também podem precisar ser transformados. Por exemplo, se a distribuição do alvo tiver uma cauda pesada, você pode optar por substituir o alvo por seu logaritmo. Mas, se fizer isso, o modelo de regressão agora preverá o log do valor mediano da casa, e não o valor mediano da casa em si. Você precisará calcular o exponencial da previsão do modelo se quiser o valor mediano da casa previsto.

Felizmente, a maioria dos transformadores do Scikit-Learn possui um método `inverse_transform()`, facilitando o cálculo da inversa de suas transformações. Por exemplo, o seguinte código mostra como escalonar os rótulos usando um `StandardScaler` (assim como fizemos para as entradas), treinar um modelo de regressão linear simples nos rótulos escalonados resultantes e usá-lo para fazer previsões em alguns novos dados, que transformamos de volta para a escala original usando o método `inverse_transform()` do escalonador treinado. Observe que convertemos os rótulos de uma Série do Pandas para um DataFrame, já que o `StandardScaler` espera entradas 2D. Além disso, neste exemplo, treinamos o modelo em uma única característica de entrada bruta (renda mediana), por simplicidade:

```python
from sklearn.linear_model import LinearRegression

target_scaler = StandardScaler()
scaled_labels = target_scaler.fit_transform(housing_labels.to_frame())

model = LinearRegression()
model.fit(housing[["median_income"]], scaled_labels)
some_new_data = housing[["median_income"]].iloc[:5]  # simula novos dados

scaled_predictions = model.predict(some_new_data)
predictions = target_scaler.inverse_transform(scaled_predictions)
```

---

Isso funciona bem, mas uma opção mais simples é usar um `TransformedTargetRegressor`. Basta construí-lo, fornecendo o modelo de regressão e o transformador de rótulos, e então ajustá-lo no conjunto de treinamento, usando os rótulos originais não escalonados. Ele usará automaticamente o transformador para escalonar os rótulos e treinar o modelo de regressão nos rótulos escalonados resultantes, assim como fizemos anteriormente. Então, quando quisermos fazer uma previsão, ele chamará o método `predict()` do modelo de regressão e usará o método `inverse_transform()` do escalonador para produzir a previsão:

```python
from sklearn.compose import TransformedTargetRegressor

model = TransformedTargetRegressor(LinearRegression(),
    transformer=StandardScaler())
model.fit(housing[["median_income"]], housing_labels)
predictions = model.predict(some_new_data)
```

---

### Transformadores Personalizados

Embora o Scikit-Learn forneça muitos transformadores úteis, você precisará escrever os seus próprios para tarefas como transformações personalizadas, operações de limpeza ou combinação de atributos específicos.

Para transformações que não exigem nenhum treinamento, você pode simplesmente escrever uma função que recebe um array NumPy como entrada e retorna o array transformado. Por exemplo, como discutido na seção anterior, muitas vezes é uma boa ideia transformar características com distribuições de cauda pesada substituindo-as por seu logaritmo (assumindo que a característica seja positiva e a cauda esteja à direita). Vamos criar um transformador de log e aplicá-lo à característica de população:

```python
from sklearn.preprocessing import FunctionTransformer

log_transformer = FunctionTransformer(np.log, inverse_func=np.exp)
log_pop = log_transformer.transform(housing[["population"]])
```

---

O argumento `inverse_func` é opcional. Ele permite que você especifique uma função de transformação inversa, por exemplo, se você planeja usar seu transformador em um `TransformedTargetRegressor`.

Sua função de transformação pode receber hiperparâmetros como argumentos adicionais. Por exemplo, veja como criar um transformador que calcula a mesma medida de similaridade RBF Gaussiana que fizemos anteriormente:

```python
rbf_transformer = FunctionTransformer(rbf_kernel,
    kw_args=dict(Y=[[35.]], gamma=0.1))
age_simil_35 = rbf_transformer.transform(housing[["housing_median_age"]])
```

---

Observe que não há função inversa para o kernel RBF, já que sempre há dois valores a uma determinada distância de um ponto fixo (exceto na distância 0). Além disso, observe que `rbf_kernel()` não trata as características separadamente. Se você passar um array com duas características, ele medirá a distância 2D (Euclidiana) para medir a similaridade. Por exemplo, veja como adicionar uma característica que medirá a similaridade geográfica entre cada distrito e São Francisco:

```python
sf_coords = 37.7749, -122.41
sf_transformer = FunctionTransformer(rbf_kernel,
    kw_args=dict(Y=[sf_coords], gamma=0.1))
sf_simil = sf_transformer.transform(housing[["latitude", "longitude"]])
```

---

Transformadores personalizados também são úteis para combinar características. Por exemplo, aqui está um `FunctionTransformer` que calcula a razão entre as características de entrada 0 e 1:

```python
>>> ratio_transformer = FunctionTransformer(lambda x: X[:, [0]] / X[:, [1]])
>>> ratio_transformer.transform(np.array([[1., 2.], [3., 4.]]))
array([[0.5 ],
    [0.75]])
```

---

O `FunctionTransformer` é muito útil, mas e se você quiser que seu transformador seja treinável, aprendendo alguns parâmetros no método `fit()` e usando-os posteriormente no método `transform()`? Para isso, você precisa escrever uma classe personalizada. O Scikit-Learn depende de **duck typing**, então essa classe não precisa herdar de nenhuma classe base específica. Tudo o que ela precisa são três métodos: `fit()` (que deve retornar `self`), `transform()` e `fit_transform()`.

Você pode obter `fit_transform()` de graça simplesmente adicionando `TransformerMixin` como uma classe base: a implementação padrão apenas chamará `fit()` e, em seguida, `transform()`. Se você adicionar `BaseEstimator` como uma classe base (e evitar usar `*args` e `**kwargs` no construtor), também obterá dois métodos extras: `get_params()` e `set_params()`. Eles serão úteis para ajuste automático de hiperparâmetros.

Por exemplo, aqui está um transformador personalizado que age de forma muito semelhante ao `StandardScaler`:

```python
from sklearn.base import BaseEstimator, TransformerMixin
from sklearn.utils.validation import check_array, check_is_fitted

class StandardScalerClone(BaseEstimator, TransformerMixin):
    def __init__(self, with_mean=True):  # sem *args ou **kwargs!
        self.with_mean = with_mean

    def fit(self, X, y=None):  # y é necessário mesmo que não o usemos
        X = check_array(X)  # verifica se X é um array com valores float finitos
        self.mean_ = X.mean(axis=0)
        self.scale_ = X.std(axis=0)
        self.n_features_in_ = X.shape[1]  # todo estimador armazena isso em fit()
        return self  # sempre retorne self!

    def transform(self, X):
        check_is_fitted(self)  # procura por atributos aprendidos (com _ no final)
        X = check_array(X)
        assert self.n_features_in_ == X.shape[1]
        if self.with_mean:
            X = X - self.mean_
        return X / self.scale_
```

---

Aqui estão algumas coisas a serem observadas:

- O pacote `sklearn.utils.validation` contém várias funções que podemos usar para validar as entradas. Para simplificar, vamos pular esses testes no restante deste livro, mas o código de produção deve tê-los.
- Os pipelines do Scikit-Learn exigem que o método `fit()` tenha dois argumentos, `X` e `y`, e é por isso que precisamos do argumento `y=None`, mesmo que não o usemos.
- Todos os estimadores do Scikit-Learn definem `n_features_in_` no método `fit()` e garantem que os dados passados para `transform()` ou `predict()` tenham esse número de características.
- O método `fit()` deve retornar `self`.
- Esta implementação não está 100% completa: todos os estimadores devem definir `feature_names_in_` no método `fit()` quando recebem um DataFrame. Além disso, todos os transformadores devem fornecer um método `get_feature_names_out()`, bem como um método `inverse_transform()` quando sua transformação puder ser revertida. Veja o último exercício no final deste capítulo para mais detalhes.

Um transformador personalizado pode (e muitas vezes o faz) usar outros estimadores em sua implementação. Por exemplo, o código a seguir demonstra um transformador personalizado que usa um clusterizador `KMeans` no método `fit()` para identificar os principais clusters nos dados de treinamento e, em seguida, usa `rbf_kernel()` no método `transform()` para medir o quão semelhante cada amostra é a cada centro de cluster:

```python
from sklearn.cluster import KMeans

class ClusterSimilarity(BaseEstimator, TransformerMixin):
    def __init__(self, n_clusters=10, gamma=1.0, random_state=None):
        self.n_clusters = n_clusters
        self.gamma = gamma
        self.random_state = random_state

    def fit(self, X, y=None, sample_weight=None):
        self.kmeans_ = KMeans(self.n_clusters, random_state=self.random_state)
        self.kmeans_.fit(X, sample_weight=sample_weight)
        return self  # sempre retorne self!

    def transform(self, X):
        return rbf_kernel(X, self.kmeans_.cluster_centers_, gamma=self.gamma)

    def get_feature_names_out(self, names=None):
        return [f"Cluster {i} similarity" for i in range(self.n_clusters)]
```

---

### DICA

Você pode verificar se seu estimador personalizado respeita a API do Scikit-Learn passando uma instância para `check_estimator()` do pacote `sklearn.utils.estimator_checks`. Para a API completa, confira *https://scikit-learn.org/stable/developers*.

Como você verá no Capítulo 9, o **k-means** é um algoritmo de clustering que localiza clusters nos dados. O número de clusters que ele procura é controlado pelo hiperparâmetro `n_clusters`. Após o treinamento, os centros dos clusters estão disponíveis via o atributo `cluster_centers_`. O método `fit()` do `KMeans` suporta um argumento opcional `sample_weight`, que permite ao usuário especificar os pesos relativos das amostras. O k-means é um algoritmo estocástico, o que significa que ele depende de aleatoriedade para localizar os clusters, então, se você quiser resultados reproduzíveis, deve definir o parâmetro `random_state`. Como você pode ver, apesar da complexidade da tarefa, o código é bastante direto. Agora vamos usar este transformador personalizado:

```python
cluster_simil = ClusterSimilarity(n_clusters=10, gamma=1., random_state=42)
similarities = cluster_simil.fit_transform(housing[["latitude", "longitude"]], sample_weight=housing_labels)
```

---

Este código cria um transformador `ClusterSimilarity`, definindo o número de clusters como 10. Em seguida, ele chama `fit_transform()` com a latitude e longitude de cada distrito no conjunto de treinamento, ponderando cada distrito pelo valor mediano da casa. O transformador usa k-means para localizar os clusters e, em seguida, mede a similaridade RBF Gaussiana entre cada distrito e todos os 10 centros de cluster. O resultado é uma matriz com uma linha por distrito e uma coluna por cluster. Vamos olhar as três primeiras linhas, arredondando para duas casas decimais:

```python
>>> similarities[:3].round(2)
array([[0. , 0.14, 0. , 0. , 0. , 0.08, 0. , 0.99, 0. , 0.6 ],
    [0.63, 0. , 0.99, 0. , 0. , 0. , 0.04, 0. , 0.11, 0. ],
    [0. , 0.29, 0. , 0. , 0.01, 0.44, 0. , 0.7 , 0. , 0.3 ]])
```

---

A Figura 2-19 mostra os 10 centros de cluster encontrados pelo k-means. Os distritos são coloridos de acordo com sua similaridade geográfica ao centro de cluster mais próximo. Como você pode ver, a maioria dos clusters está localizada em áreas altamente populosas e caras.

---

### Pipelines de Transformação

Como você pode ver, há muitas etapas de transformação de dados que precisam ser executadas na ordem correta. Felizmente, o Scikit-Learn fornece a classe `Pipeline` para ajudar com essas sequências de transformações. Aqui está um pequeno pipeline para atributos numéricos, que primeiro imputará e depois escalonará as características de entrada:

```python
from sklearn.pipeline import Pipeline

num_pipeline = Pipeline([
    ("impute", SimpleImputer(strategy="median")),
    ("standardize", StandardScaler()),
])
```

---

O construtor `Pipeline` recebe uma lista de pares nome/estimador (tuplas de 2 elementos) que definem uma sequência de etapas. Os nomes podem ser qualquer coisa que você quiser, desde que sejam únicos e não contenham sublinhados duplos (`__`). Eles serão úteis mais tarde, quando discutirmos o ajuste de hiperparâmetros. Os estimadores devem ser todos transformadores (ou seja, devem ter um método `fit_transform()`), exceto o último, que pode ser qualquer coisa: um transformador, um preditor ou qualquer outro tipo de estimador.

---

### DICA

Em um notebook Jupyter, se você importar `sklearn` e executar `sklearn.set_config(display="diagram")`, todos os estimadores do Scikit-Learn serão renderizados como diagramas interativos. Isso é particularmente útil para visualizar pipelines. Para visualizar `num_pipeline`, execute uma célula com `num_pipeline` como a última linha. Clicar em um estimador mostrará mais detalhes.

Se você não quiser nomear os transformadores, pode usar a função `make_pipeline()`; ela recebe os transformadores como argumentos posicionais e cria um `Pipeline` usando os nomes das classes dos transformadores, em letras minúsculas e sem sublinhados (por exemplo, `"simpleinputer"`):

```python
from sklearn.pipeline import make_pipeline

num_pipeline = make_pipeline(SimpleImputer(strategy="median"), StandardScaler())
```

Se vários transformadores tiverem o mesmo nome, um índice será anexado aos seus nomes (por exemplo, `"foo-1"`, `"foo-2"`, etc.).

Quando você chama o método `fit()` do pipeline, ele chama `fit_transform()` sequencialmente em todos os transformadores, passando a saída de cada chamada como parâmetro para a próxima chamada até chegar ao estimador final, para o qual ele apenas chama o método `fit()`.

O pipeline expõe os mesmos métodos que o estimador final. Neste exemplo, o último estimador é um `StandardScaler`, que é um transformador, então o pipeline também age como um transformador. Se você chamar o método `transform()` do pipeline, ele aplicará todas as transformações aos dados. Se o último estimador fosse um preditor em vez de um transformador, o pipeline teria um método `predict()` em vez de `transform()`. Chamá-lo aplicaria todas as transformações aos dados e passaria o resultado para o método `predict()` do preditor.

Vamos chamar o método `fit_transform()` do pipeline e olhar as duas primeiras linhas da saída, arredondadas para duas casas decimais:

```python
>>> housing_num_prepared = num_pipeline.fit_transform(housing_num)
>>> housing_num_prepared[:2].round(2)
array([[-1.42, 1.01, 1.86, 0.31, 1.37, 0.14, 1.39, -0.94],
    [ 0.6 , -0.7 , 0.91, -0.31, -0.44, -0.69, -0.37, 1.17]])
```

---

Como vimos anteriormente, se você quiser recuperar um DataFrame organizado, pode usar o método `get_feature_names_out()` do pipeline:

```python
df_housing_num_prepared = pd.DataFrame(
    housing_num_prepared, columns=num_pipeline.get_feature_names_out(),
    index=housing_num.index)
```

---

Pipelines suportam indexação; por exemplo, `pipeline[1]` retorna o segundo estimador no pipeline, e `pipeline[:-1]` retorna um objeto `Pipeline` contendo todos os estimadores, exceto o último. Você também pode acessar os estimadores via o atributo `steps`, que é uma lista de pares nome/estimador, ou via o atributo `named_steps`, que mapeia os nomes para os estimadores. Por exemplo, `num_pipeline["simpleinputer"]` retorna o estimador chamado `"simpleinputer"`.

Até agora, lidamos com as colunas categóricas e numéricas separadamente. Seria mais conveniente ter um único transformador capaz de lidar com todas as colunas, aplicando as transformações apropriadas a cada coluna. Para isso, você pode usar um `ColumnTransformer`. Por exemplo, o seguinte `ColumnTransformer` aplicará `num_pipeline` (o que acabamos de definir) aos atributos numéricos e `cat_pipeline` ao atributo categórico:

```python
from sklearn.compose import ColumnTransformer

num_attribs = ["longitude", "latitude", "housing_median_age", "total_rooms",
    "total_bedrooms", "population", "households", "median_income"]
cat_attribs = ["ocean_proximity"]

cat_pipeline = make_pipeline(
    SimpleImputer(strategy="most_frequent"),
    OneHotEncoder(handle_unknown="ignore"))
preprocessing = ColumnTransformer([
    ("num", num_pipeline, num_attribs),
    ("cat", cat_pipeline, cat_attribs),
])
```

---

Primeiro, importamos a classe `ColumnTransformer`, depois definimos a lista de nomes de colunas numéricas e categóricas e construímos um pipeline simples para atributos categóricos. Por fim, construímos um `ColumnTransformer`. Seu construtor requer uma lista de trios (tuplas de 3 elementos), cada um contendo um nome (que deve ser único e não conter sublinhados duplos), um transformador e uma lista de nomes (ou índices) das colunas às quais o transformador deve ser aplicado.

---

### DICA

Em vez de usar um transformador, você pode especificar a string `"drop"` se quiser que as colunas sejam descartadas, ou pode especificar `"passthrough"` se quiser que as colunas permaneçam inalteradas. Por padrão, as colunas restantes (ou seja, aquelas que não foram listadas) serão descartadas, mas você pode definir o hiperparâmetro `remainder` para qualquer transformador (ou para `"passthrough"`) se quiser que essas colunas sejam tratadas de forma diferente.

Como listar todos os nomes das colunas não é muito conveniente, o Scikit-Learn fornece uma função `make_column_selector()` que retorna uma função seletora que você pode usar para selecionar automaticamente todas as características de um determinado tipo, como numéricas ou categóricas. Você pode passar essa função seletora para o `ColumnTransformer` em vez de nomes ou índices de colunas. Além disso, se você não se importa em nomear os transformadores, pode usar `make_column_transformer()`, que escolhe os nomes para você, assim como `make_pipeline()` faz. Por exemplo, o seguinte código cria o mesmo `ColumnTransformer` que antes, exceto que os transformadores são automaticamente nomeados como `"pipeline-1"` e `"pipeline-2"` em vez de `"num"` e `"cat"`:

```python
from sklearn.compose import make_column_selector, make_column_transformer

preprocessing = make_column_transformer(
    (num_pipeline, make_column_selector(dtype_include=np.number)),
    (cat_pipeline, make_column_selector(dtype_include=object)),
)
```

---

Agora estamos prontos para aplicar este `ColumnTransformer` aos dados de habitação:

```python
housing_prepared = preprocessing.fit_transform(housing)
```

---

Ótimo! Temos um pipeline de pré-processamento que pega todo o conjunto de treinamento e aplica cada transformador às colunas apropriadas, depois concatena as colunas transformadas horizontalmente (os transformadores nunca devem alterar o número de linhas). Mais uma vez, isso retorna um array NumPy, mas você pode obter os nomes das colunas usando `preprocessing.get_feature_names_out()` e envolver os dados em um DataFrame organizado, como fizemos antes.

---

### NOTA

O `OneHotEncoder` retorna uma matriz esparsa e o `num_pipeline` retorna uma matriz densa. Quando há uma mistura de matrizes esparsas e densas, o `ColumnTransformer` estima a densidade da matriz final (ou seja, a proporção de células não nulas) e retorna uma matriz esparsa se a densidade for menor que um limite especificado (por padrão, `sparse_threshold=0.3`). Neste exemplo, ele retorna uma matriz densa.

Seu projeto está indo muito bem e você está quase pronto para treinar alguns modelos! Agora você quer criar um único pipeline que realizará todas as transformações que você experimentou até agora. Vamos recapitular o que o pipeline fará e por quê:

---

- Valores ausentes em características numéricas serão imputados substituindo-os pela mediana, já que a maioria dos algoritmos de ML não espera valores ausentes. Em características categóricas, os valores ausentes serão substituídos pela categoria mais frequente.
- A característica categórica será codificada em one-hot, já que a maioria dos algoritmos de ML só aceita entradas numéricas.
- Algumas características de razão serão calculadas e adicionadas: `bedrooms_ratio`, `rooms_per_house` e `people_per_house`. Espera-se que elas se correlacionem melhor com o valor mediano da casa e, assim, ajudem os modelos de ML.
- Algumas características de similaridade de cluster também serão adicionadas. Elas provavelmente serão mais úteis para o modelo do que latitude e longitude.
- Características com cauda longa serão substituídas por seu logaritmo, já que a maioria dos modelos prefere características com distribuições aproximadamente uniformes ou Gaussianas.
- Todas as características numéricas serão padronizadas, já que a maioria dos algoritmos de ML prefere que todas as características tenham aproximadamente a mesma escala.

O código que constrói o pipeline para fazer tudo isso deve parecer familiar para você agora:

```python
def column_ratio(X):
    return X[:, [0]] / X[:, [1]]

def ratio_name(function_transformer, feature_names_in):
    return ["ratio"]  # nomes das características de saída

def ratio_pipeline():
    return make_pipeline(
        SimpleImputer(strategy="median"),
        FunctionTransformer(column_ratio, feature_names_out=ratio_name),
        StandardScaler())

log_pipeline = make_pipeline(
    SimpleImputer(strategy="median"),
    FunctionTransformer(np.log, feature_names_out="one-to-one"),
    StandardScaler())
cluster_simil = ClusterSimilarity(n_clusters=10, gamma=1., random_state=42)

default_num_pipeline = make_pipeline(SimpleImputer(strategy="median"),
    StandardScaler())
preprocessing = ColumnTransformer([
    ("bedrooms", ratio_pipeline(), ["total_bedrooms", "total_rooms"],
    ("rooms_per_house", ratio_pipeline(), ["total_rooms", "households"],
    ("people_per_house", ratio_pipeline(), ["population", "households"],
    ("log", log_pipeline, ["total_bedrooms", "total_rooms", "population",
        "households", "median_income"],
    ("geo", cluster_simil, ["latitude", "longitude"],
    ("cat", cat_pipeline, make_column_selector(dtype_include=object)),
],
    remainder=default_num_pipeline)  # uma coluna restante: housing_median_age
```

---

Se você executar este `ColumnTransformer`, ele realizará todas as transformações e retornará um array NumPy com 24 características:

```python
>>> housing_prepared = preprocessing.fit_transform(housing)
>>> housing_prepared.shape
(16512, 24)
>>> preprocessing.get_feature_names_out()
array(['bedrooms__ratio', 'rooms_per_house__ratio',
    'people_per_house__ratio', 'log__total_bedrooms',
    'log__total_rooms', 'log__population', 'log__households',
    'log__median_income', 'geo__cluster 0 similarity', [...],
    'geo__cluster 9 similarity', 'cat__ocean_proximity_<1H OCEAN',
    'cat__ocean_proximity_INLAND', 'cat__ocean_proximity_ISLAND',
    'cat__ocean_proximity_NEAR BAY', 'cat__ocean_proximity_NEAR OCEAN',
    'remainder__housing_median_age'], dtype=object)
```

---

### Selecionar e Treinar um Modelo

Finalmente! Você enquadrou o problema, obteve os dados e os explorou, amostrou um conjunto de treinamento e um conjunto de teste, e escreveu um pipeline de pré-processamento para limpar e preparar automaticamente seus dados para algoritmos de aprendizado de máquina. Agora você está pronto para selecionar e treinar um modelo de aprendizado de máquina.

---

### Treinar e Avaliar no Conjunto de Treinamento

A boa notícia é que, graças a todas essas etapas anteriores, as coisas agora serão fáceis! Você decide treinar um modelo de regressão linear muito básico para começar:

```python
from sklearn.linear_model import LinearRegression

lin_reg = make_pipeline(preprocessing, LinearRegression())
lin_reg.fit(housing, housing_labels)
```

---

Pronto! Agora você tem um modelo de regressão linear funcionando. Você o testa no conjunto de treinamento, observando as cinco primeiras previsões e comparando-as com os rótulos:

```python
>>> housing_predictions = lin_reg.predict(housing)
>>> housing_predictions[:5].round(-2)  # -2 = arredondado para a centena mais próxima
array([243700., 372400., 128800., 94400., 328300.])
>>> housing_labels.iloc[:5].values
array([458300., 483800., 101700., 96100., 361800.])
```

---

Bem, ele funciona, mas nem sempre: a primeira previsão está muito longe (mais de $200.000!), enquanto as outras previsões são melhores: duas estão erradas em cerca de 25%, e duas estão erradas em menos de 10%. Lembre-se de que você escolheu usar o RMSE como sua medida de desempenho, então você quer medir o RMSE desse modelo de regressão em todo o conjunto de treinamento usando a função `mean_squared_error()` do Scikit-Learn, com o argumento `squared` definido como `False`:

```python
>>> from sklearn.metrics import mean_squared_error
>>> lin_rmse = mean_squared_error(housing_labels, housing_predictions,
...     squared=False)
...
>>> lin_rmse
68687.89176589991
```

---

Isso é melhor do que nada, mas claramente não é uma pontuação excelente: os valores medianos das casas na maioria dos distritos variam entre $120.000 e $265.000, então um erro de previsão típico de $68.628 não é muito satisfatório. Este é um exemplo de um modelo que está **subajustando** os dados de treinamento. Quando isso acontece, pode significar que as características não fornecem informações suficientes para fazer boas previsões, ou que o modelo não é poderoso o suficiente. Como vimos no capítulo anterior, as principais maneiras de corrigir o subajuste são selecionar um modelo mais poderoso, alimentar o algoritmo de treinamento com melhores características ou reduzir as restrições no modelo. Este modelo não é regularizado, o que descarta a última opção. Você poderia tentar adicionar mais características, mas primeiro quer tentar um modelo mais complexo para ver como ele se sai.

Você decide tentar um `DecisionTreeRegressor`, já que este é um modelo bastante poderoso capaz de encontrar relações não lineares complexas nos dados (as árvores de decisão serão apresentadas em mais detalhes no Capítulo 6):

```python
from sklearn.tree import DecisionTreeRegressor

tree_reg = make_pipeline(preprocessing,
    DecisionTreeRegressor(random_state=42))
tree_reg.fit(housing, housing_labels)
```

---

Agora que o modelo está treinado, você o avalia no conjunto de treinamento:

```python
>>> housing_predictions = tree_reg.predict(housing)
>>> tree_rmse = mean_squared_error(housing_labels, housing_predictions,
...     squared=False)
...
>>> tree_rmse
0.0
```

---

Espere, o quê!? Nenhum erro? Esse modelo realmente poderia ser absolutamente perfeito? Claro, é muito mais provável que o modelo tenha **sobreajustado** os dados. Como você pode ter certeza? Como você viu anteriormente, você não quer tocar no conjunto de teste até estar pronto para lançar um modelo no qual confia, então você precisa usar parte do conjunto de treinamento para treinar e parte para validação do modelo.

---

### Avaliação Melhor Usando Validação Cruzada

Uma maneira de avaliar o modelo de árvore de decisão seria usar a função `train_test_split()` para dividir o conjunto de treinamento em um conjunto de treinamento menor e um conjunto de validação, depois treinar seus modelos no conjunto de treinamento menor e avaliá-los no conjunto de validação. É um pouco trabalhoso, mas nada muito difícil, e funcionaria razoavelmente bem.

Uma ótima alternativa é usar o recurso de **validação cruzada k-fold** do Scikit-Learn. O código a seguir divide aleatoriamente o conjunto de treinamento em 10 subconjuntos não sobrepostos chamados **folds**, depois treina e avalia o modelo de árvore de decisão 10 vezes, escolhendo um fold diferente para avaliação a cada vez e usando os outros 9 folds para treinamento. O resultado é um array contendo as 10 pontuações de avaliação:

```python
from sklearn.model_selection import cross_val_score

tree_rmses = -cross_val_score(tree_reg, housing, housing_labels,
    scoring="neg_root_mean_squared_error", cv=10)
```

---

### AVISO

Os recursos de validação cruzada do Scikit-Learn esperam uma função de utilidade (quanto maior, melhor) em vez de uma função de custo (quanto menor, melhor), então a função de pontuação é na verdade o oposto do RMSE. É um valor negativo, então você precisa inverter o sinal da saída para obter as pontuações de RMSE.

Vamos olhar os resultados:

```python
>>> pd.Series(tree_rmses).describe()
count    10.000000
mean    66868.027288
std    2060.966425
min    63649.536493
25%    65338.078316
50%    66801.953094
75%    68229.934454
max    70094.778246
dtype: float64
```

---

Agora a árvore de decisão não parece tão boa quanto parecia antes. Na verdade, ela parece performar quase tão mal quanto o modelo de regressão linear! Observe que a validação cruzada permite que você obtenha não apenas uma estimativa do desempenho do seu modelo, mas também uma medida de quão precisa é essa estimativa (ou seja, seu desvio padrão). A árvore de decisão tem um RMSE de cerca de 66.868, com um desvio padrão de cerca de 2.061. Você não teria essa informação se usasse apenas um conjunto de validação. Mas a validação cruzada tem o custo de treinar o modelo várias vezes, então nem sempre é viável.

Se você calcular a mesma métrica para o modelo de regressão linear, descobrirá que o RMSE médio é 69.858 e o desvio padrão é 4.182. Portanto, o modelo de árvore de decisão parece performar um pouco melhor do que o modelo linear, mas a diferença é mínima devido ao grave sobreajuste. Sabemos que há um problema de sobreajuste porque o erro de treinamento é baixo (na verdade, zero), enquanto o erro de validação é alto.

Vamos tentar um último modelo agora: o `RandomForestRegressor`. Como você verá no Capítulo 7, as florestas aleatórias funcionam treinando muitas árvores de decisão em subconjuntos aleatórios das características e, em seguida, calculando a média de suas previsões. Esses modelos compostos por muitos outros modelos são chamados de **ensembles**: eles são capazes de impulsionar o desempenho do modelo subjacente (neste caso, árvores de decisão). O código é muito semelhante ao anterior:

```python
from sklearn.ensemble import RandomForestRegressor

forest_reg = make_pipeline(preprocessing,
    RandomForestRegressor(random_state=42))
forest_rmses = -cross_val_score(forest_reg, housing, housing_labels,
    scoring="neg_root_mean_squared_error", cv=10)
```

---

Vamos olhar as pontuações:

```python
>>> pd.Series(forest_rmses).describe()
count    10.000000
mean    47019.561281
std    1033.957120
min    45458.112527
25%    46464.031184
50%    46967.596354
75%    47325.694987
max    49243.765795
dtype: float64
```

---

Uau, isso é muito melhor: as florestas aleatórias realmente parecem muito promissoras para essa tarefa! No entanto, se você treinar uma `RandomForest` e medir o RMSE no conjunto de treinamento, encontrará aproximadamente 17.474: isso é muito menor, o que significa que ainda há bastante sobreajuste acontecendo. Possíveis soluções são simplificar o modelo, restringi-lo (ou seja, regularizá-lo) ou obter muito mais dados de treinamento. Antes de mergulhar muito mais fundo nas florestas aleatórias, no entanto, você deve experimentar muitos outros modelos de várias categorias de algoritmos de aprendizado de máquina (por exemplo, várias máquinas de vetores de suporte com diferentes kernels e possivelmente uma rede neural), sem gastar muito tempo ajustando os hiperparâmetros. O objetivo é selecionar alguns (de dois a cinco) modelos promissores.

---

### Ajustar o Modelo

Vamos supor que agora você tenha uma lista curta de modelos promissores. Agora você precisa ajustá-los. Vamos ver algumas maneiras de fazer isso.

---

### Busca em Grade

Uma opção seria ajustar manualmente os hiperparâmetros até encontrar uma ótima combinação de valores. Isso seria um trabalho muito tedioso, e você pode não ter tempo para explorar muitas combinações.

Em vez disso, você pode usar a classe `GridSearchCV` do Scikit-Learn para fazer a busca por você. Tudo o que você precisa fazer é dizer a ele quais hiperparâmetros você quer que ele experimente e quais valores tentar, e ele usará validação cruzada para avaliar todas as combinações possíveis de valores de hiperparâmetros. Por exemplo, o código a seguir procura a melhor combinação de valores de hiperparâmetros para o `RandomForestRegressor`:

```python
from sklearn.model_selection import GridSearchCV

full_pipeline = Pipeline([
    ("preprocessing", preprocessing),
    ("random_forest", RandomForestRegressor(random_state=42)),
])
param_grid = [
    {'preprocessing__geo__n_clusters': [5, 8, 10],
     'random_forest__max_features': [4, 6, 8]},
    {'preprocessing__geo__n_clusters': [10, 15],
     'random_forest__max_features': [6, 8, 10]},
]
grid_search = GridSearchCV(full_pipeline, param_grid, cv=3,
    scoring='neg_root_mean_squared_error')
grid_search.fit(housing, housing_labels)
```

---

Observe que você pode se referir a qualquer hiperparâmetro de qualquer estimador em um pipeline, mesmo que esse estimador esteja aninhado profundamente dentro de vários pipelines e transformadores de colunas. Por exemplo, quando o Scikit-Learn vê `"preprocessing__geo__n_clusters"`, ele divide essa string nos sublinhados duplos, depois procura por um estimador chamado `"preprocessing"` no pipeline e encontra o `ColumnTransformer` de pré-processamento. Em seguida, ele procura por um transformador chamado `"geo"` dentro desse `ColumnTransformer` e encontra o transformador `ClusterSimilarity` que usamos nas características de latitude e longitude. Então ele encontra o hiperparâmetro `n_clusters` desse transformador. Da mesma forma, `random_forest__max_features` se refere ao hiperparâmetro `max_features` do estimador chamado `"random_forest"`, que é, claro, o modelo `RandomForest` (o hiperparâmetro `max_features` será explicado no Capítulo 7).

---

### DICA

Envolver etapas de pré-processamento em um pipeline do Scikit-Learn permite que você ajuste os hiperparâmetros de pré-processamento junto com os hiperparâmetros do modelo. Isso é uma coisa boa, pois eles frequentemente interagem. Por exemplo, talvez aumentar `n_clusters` exija aumentar `max_features` também. Se o ajuste dos transformadores do pipeline for computacionalmente caro, você pode definir o hiperparâmetro `memory` do pipeline para o caminho de um diretório de cache: quando você ajustar o pipeline pela primeira vez, o Scikit-Learn salvará os transformadores ajustados nesse diretório. Se você ajustar o pipeline novamente com os mesmos hiperparâmetros, o Scikit-Learn apenas carregará os transformadores em cache.

---

Há dois dicionários neste `param_grid`, então o `GridSearchCV` primeiro avaliará todas as \(3 \times 3 = 9\) combinações de valores de hiperparâmetros `n_clusters` e `max_features` especificadas no primeiro dicionário, depois tentará todas as \(2 \times 3 = 6\) combinações de valores de hiperparâmetros no segundo dicionário. Então, no total, a busca em grade explorará \(9 + 6 = 15\) combinações de valores de hiperparâmetros, e treinará o pipeline 3 vezes por combinação, já que estamos usando validação cruzada de 3 folds. Isso significa que haverá um total de \(15 \times 3 = 45\) rodadas de treinamento! Pode demorar um pouco, mas quando terminar, você pode obter a melhor combinação de parâmetros assim:

```python
>>> grid_search.best_params_
{'preprocessing__geo__n_clusters': 15, 'random_forest__max_features': 6}
```

---

Neste exemplo, o melhor modelo é obtido definindo `n_clusters` como 15 e `max_features` como 8.

---

### DICA

Como 15 é o valor máximo que foi avaliado para `n_clusters`, você provavelmente deve tentar pesquisar novamente com valores mais altos; a pontuação pode continuar a melhorar.

---

Você pode acessar o melhor estimador usando `grid_search.best_estimator_`. Se o `GridSearchCV` for inicializado com `refit=True` (que é o padrão), então, depois de encontrar o melhor estimador usando validação cruzada, ele o reajustará em todo o conjunto de treinamento. Isso geralmente é uma boa ideia, pois alimentá-lo com mais dados provavelmente melhorará seu desempenho.

As pontuações de avaliação estão disponíveis usando `grid_search.cv_results_`. Este é um dicionário, mas se você envolvê-lo em um DataFrame, obterá uma lista organizada de todas as pontuações de teste para cada combinação de hiperparâmetros e para cada divisão de validação cruzada, bem como a pontuação média de teste em todas as divisões:

```python
>>> cv_res = pd.DataFrame(grid_search.cv_results_)
>>> cv_res.sort_values(by="mean_test_score", ascending=False, inplace=True)
>>> [...]  # mudar os nomes das colunas para caber nesta página e mostrar rmse = -score
>>> cv_res.head()  # nota: a 1ª coluna é o ID da linha
    n_clusters  max_features  split0_test_score  split1_test_score  split2_test_score  mean_test_rmse
12          15             6            43460            43919            44748            44042
13          15             8            44132            44075            45010            44406
14          15            10            44374            44286            45316            44659
7           10             6            44683            44655            45657            44999
9           10             6            44683            44655            45657            44999
```

---

A pontuação média de RMSE de teste para o melhor modelo é 44.042, o que é melhor do que a pontuação que você obteve anteriormente usando os valores padrão dos hiperparâmetros (que foi 47.019). Parabéns, você ajustou com sucesso seu melhor modelo!

---

### Busca Aleatória

A abordagem de busca em grade é boa quando você está explorando relativamente poucas combinações, como no exemplo anterior, mas o `RandomizedSearchCV` é frequentemente preferível, especialmente quando o espaço de busca de hiperparâmetros é grande. Essa classe pode ser usada da mesma forma que a classe `GridSearchCV`, mas, em vez de tentar todas as combinações possíveis, ela avalia um número fixo de combinações, selecionando um valor aleatório para cada hiperparâmetro em cada iteração. Isso pode parecer surpreendente, mas essa abordagem tem vários benefícios:

- Se alguns de seus hiperparâmetros são contínuos (ou discretos, mas com muitos valores possíveis), e você deixar a busca aleatória rodar por, digamos, 1.000 iterações, ela explorará 1.000 valores diferentes para cada um desses hiperparâmetros, enquanto a busca em grade exploraria apenas os poucos valores que você listou para cada um.
- Suponha que um hiperparâmetro não faça muita diferença, mas você ainda não sabe disso. Se ele tiver 10 valores possíveis e você adicioná-lo à sua busca em grade, o treinamento levará 10 vezes mais tempo. Mas se você adicioná-lo a uma busca aleatória, isso não fará diferença.
- Se houver 6 hiperparâmetros para explorar, cada um com 10 valores possíveis, a busca em grade não oferece outra escolha além de treinar o modelo um milhão de vezes, enquanto a busca aleatória pode rodar por qualquer número de iterações que você escolher.

Para cada hiperparâmetro, você deve fornecer uma lista de valores possíveis ou uma distribuição de probabilidade:

```python
from sklearn.model_selection import RandomizedSearchCV
from scipy.stats import randint

param_distribs = {'preprocessing__geo__n_clusters': randint(low=3, high=50),
    'random_forest__max_features': randint(low=2, high=20)}

rnd_search = RandomizedSearchCV(
    full_pipeline, param_distributions=param_distribs, n_iter=10, cv=3,
    scoring='neg_root_mean_squared_error', random_state=42)

rnd_search.fit(housing, housing_labels)
```

---

O Scikit-Learn também tem as classes de busca de hiperparâmetros `HalvingRandomSearchCV` e `HalvingGridSearchCV`. Seu objetivo é usar os recursos computacionais de forma mais eficiente, seja para treinar mais rápido ou para explorar um espaço de hiperparâmetros maior. Aqui está como elas funcionam: na primeira rodada, muitas combinações de hiperparâmetros (chamadas de "candidatos") são geradas usando a abordagem de grade ou aleatória. Esses candidatos são então usados para treinar modelos que são avaliados usando validação cruzada, como de costume. No entanto, o treinamento usa recursos limitados, o que acelera bastante essa primeira rodada. Por padrão, "recursos limitados" significa que os modelos são treinados em uma pequena parte do conjunto de treinamento. No entanto, outras limitações são possíveis, como reduzir o número de iterações de treinamento se o modelo tiver um hiperparâmetro para defini-lo. Depois que cada candidato for avaliado, apenas os melhores passam para a segunda rodada, onde recebem mais recursos para competir. Após várias rodadas, os candidatos finais são avaliados usando recursos completos. Isso pode economizar algum tempo ao ajustar hiperparâmetros.

---

### Métodos de Ensemble

Outra maneira de ajustar seu sistema é tentar combinar os modelos que performam melhor. O grupo (ou "ensemble") frequentemente performará melhor do que o melhor modelo individual — assim como as florestas aleatórias performam melhor do que as árvores de decisão individuais nas quais se baseiam — especialmente se os modelos individuais cometerem tipos muito diferentes de erros. Por exemplo, você poderia treinar e ajustar um modelo de **k-vizinhos mais próximos** (k-nearest neighbors), depois criar um modelo de ensemble que simplesmente prevê a média da previsão da floresta aleatória e da previsão desse modelo. Cobriremos esse tópico com mais detalhes no Capítulo 7.

---

### Analisando os Melhores Modelos e Seus Erros

Você frequentemente obterá bons insights sobre o problema ao inspecionar os melhores modelos. Por exemplo, o `RandomForestRegressor` pode indicar a importância relativa de cada atributo para fazer previsões precisas:

```python
>>> final_model = rnd_search.best_estimator_  # inclui o pré-processamento
>>> feature_importances = final_model["random_forest"].feature_importances_
>>> feature_importances.round(2)
array([0.07, 0.05, 0.05, 0.01, 0.01, 0.01, 0.01, 0.19, [...], 0.01])
```

---

Vamos ordenar essas pontuações de importância em ordem decrescente e exibi-las ao lado dos nomes dos atributos correspondentes:

```python
>>> sorted(zip(feature_importances,
...    final_model["preprocessing"].get_feature_names_out()),
...    reverse=True)
...
[(0.18694559869103852, 'log__median_income'),
   (0.0748194995715524, 'cat__ocean_proximity_INLAND'),
   (0.06926417748515576, 'bedrooms__ratio'),
   (0.05446998753775219, 'rooms_per_house__ratio'),
   (0.05262301809680712, 'people_per_house__ratio'),
   (0.03819415873915732, 'geo__cluster 0 similarity'),
   [...] 
   (0.00015061247730531558, 'cat__ocean_proximity_NEAR_BAY'),
   (7.301686597099842e-05, 'cat__ocean_proximity_ISLAND')]
```

---

Com essas informações, você pode querer tentar descartar algumas das características menos úteis (por exemplo, aparentemente apenas uma categoria de `ocean_proximity` é realmente útil, então você poderia tentar descartar as outras).

---

### DICA

O transformador `sklearn.feature_selection.SelectFromModel` pode descartar automaticamente as características menos úteis para você: quando você o ajusta, ele treina um modelo (normalmente uma floresta aleatória), olha para o atributo `feature_importances_` e seleciona as características mais úteis. Então, quando você chama `transform()`, ele descarta as outras características.

Você também deve olhar para os erros específicos que seu sistema comete, tentar entender por que ele os comete e o que poderia corrigir o problema: adicionar características extras ou se livrar de características não informativas, limpar outliers, etc.

Agora também é um bom momento para garantir que seu modelo não apenas funcione bem em média, mas também em todas as categorias de distritos, sejam eles rurais ou urbanos, ricos ou pobres, do norte ou do sul, minorias ou não, etc. Criar subconjuntos do seu conjunto de validação para cada categoria dá um pouco de trabalho, mas é importante: se seu modelo performar mal em uma categoria inteira de distritos, ele provavelmente não deve ser implantado até que o problema seja resolvido, ou pelo menos não deve ser usado para fazer previsões para essa categoria, pois pode fazer mais mal do que bem.

---

### Avaliar Seu Sistema no Conjunto de Teste

Depois de ajustar seus modelos por um tempo, você finalmente tem um sistema que performa suficientemente bem. Você está pronto para avaliar o modelo final no conjunto de teste. Não há nada de especial nesse processo; basta obter os preditores e os rótulos do seu conjunto de teste e executar seu `final_model` para transformar os dados e fazer previsões, depois avaliar essas previsões:

```python
X_test = strat_test_set.drop("median_house_value", axis=1)
y_test = strat_test_set["median_house_value"].copy()

final_predictions = final_model.predict(X_test)

final_rmse = mean_squared_error(y_test, final_predictions, squared=False)
print(final_rmse)  # imprime 41424.40026462184
```

---

Em alguns casos, uma estimativa pontual do erro de generalização não será suficiente para convencê-lo a lançar: e se for apenas 0,1% melhor do que o modelo atualmente em produção? Você pode querer ter uma ideia de quão precisa é essa estimativa. Para isso, você pode calcular um intervalo de confiança de 95% para o erro de generalização usando `scipy.stats.t.interval()`. Você obtém um intervalo bastante grande, de 39.275 a 43.467, e sua estimativa pontual anterior de 41.424 está aproximadamente no meio dele:

```python
>>> from scipy import stats
>>> confidence = 0.95
>>> squared_errors = (final_predictions - y_test) ** 2
>>> np.sqrt(stats.t.interval(confidence, len(squared_errors) - 1,
...     loc=squared_errors.mean(),
...     scale=stats.sem(squared_errors)))
...
array([39275.40861216, 43467.27680583])
```

---

Se você fez muito ajuste de hiperparâmetros, o desempenho geralmente será um pouco pior do que o que você mediu usando validação cruzada. Isso acontece porque seu sistema acaba ajustado para performar bem nos dados de validação e provavelmente não performará tão bem em conjuntos de dados desconhecidos. Esse não é o caso neste exemplo, já que o RMSE de teste é menor do que o RMSE de validação, mas quando isso acontecer, você deve resistir à tentação de ajustar os hiperparâmetros para fazer os números parecerem bons no conjunto de teste; as melhorias provavelmente não se generalizariam para novos dados.

Agora vem a fase de pré-lançamento do projeto: você precisa apresentar sua solução (destacando o que aprendeu, o que funcionou e o que não funcionou, quais suposições foram feitas e quais são as limitações do seu sistema), documentar tudo e criar apresentações agradáveis com visualizações claras e declarações fáceis de lembrar (por exemplo, "a renda mediana é o principal preditor dos preços das casas"). Neste exemplo de habitação na Califórnia, o desempenho final do sistema não é muito melhor do que as estimativas de preço dos especialistas, que frequentemente erravam em 30%, mas ainda pode ser uma boa ideia lançá-lo, especialmente se isso liberar algum tempo para os especialistas trabalharem em tarefas mais interessantes e produtivas.

---

### Lançar, Monitorar e Manter Seu Sistema

Perfeito, você obteve aprovação para lançar! Agora você precisa preparar sua solução para produção (por exemplo, polir o código, escrever documentação e testes, etc.). Então você pode implantar seu modelo em seu ambiente de produção. A maneira mais básica de fazer isso é simplesmente salvar o melhor modelo que você treinou, transferir o arquivo para seu ambiente de produção e carregá-lo. Para salvar o modelo, você pode usar a biblioteca `joblib` assim:

```python
import joblib
joblib.dump(final_model, "my_california_housing_model.pkl")
```

---

### DICA

Muitas vezes é uma boa ideia salvar todos os modelos que você experimentar, para que você possa voltar facilmente a qualquer modelo que desejar. Você também pode salvar as pontuações de validação cruzada e talvez as previsões reais no conjunto de validação. Isso permitirá que você compare facilmente as pontuações entre os tipos de modelos e compare os tipos de erros que eles cometem.

---

Uma vez que seu modelo é transferido para produção, você pode carregá-lo e usá-lo. Para isso, você deve primeiro importar quaisquer classes e funções personalizadas das quais o modelo depende (o que significa transferir o código para produção), depois carregar o modelo usando `joblib` e usá-lo para fazer previsões:

```python
import joblib
[...]  # importar KMeans, BaseEstimator, TransformerMixin, rbf_kernel, etc.

def column_ratio(X): [...]
def ratio_name(function_transformer, feature_names_in): [...]
class ClusterSimilarity(BaseEstimator, TransformerMixin): [...]

final_model_reloaded = joblib.load("my_california_housing_model.pkl")

new_data = [...]  # alguns novos distritos para fazer previsões
predictions = final_model_reloaded.predict(new_data)
```

---

Por exemplo, talvez o modelo seja usado dentro de um site: o usuário digitará alguns dados sobre um novo distrito e clicará no botão "Estimar Preço". Isso enviará uma consulta contendo os dados para o servidor web, que os encaminhará para sua aplicação web, e finalmente seu código simplesmente chamará o método `predict()` do modelo (você quer carregar o modelo na inicialização do servidor, em vez de toda vez que o modelo for usado). Alternativamente, você pode encapsular o modelo em um serviço web dedicado que sua aplicação web pode consultar por meio de uma API REST[13] (veja a Figura 2-20). Isso facilita a atualização do modelo para novas versões sem interromper a aplicação principal. Também simplifica a escalabilidade, já que você pode iniciar quantos serviços web forem necessários e balancear a carga das requisições vindas de sua aplicação web entre esses serviços. Além disso, permite que sua aplicação web use qualquer linguagem de programação, não apenas Python.

---

_Figura 2-20. Um modelo implantado como um serviço web e usado por uma aplicação web_

Outra estratégia popular é implantar seu modelo na nuvem, por exemplo, no **Vertex AI** do Google (anteriormente conhecido como Google Cloud AI Platform e Google Cloud ML Engine): basta salvar seu modelo usando `joblib` e enviá-lo para o **Google Cloud Storage (GCS)**, depois acessar o Vertex AI e criar uma nova versão do modelo, apontando para o arquivo no GCS. É isso! Isso lhe dá um serviço web simples que cuida do balanceamento de carga e da escalabilidade para você. Ele recebe requisições JSON contendo os dados de entrada (por exemplo, de um distrito) e retorna respostas JSON contendo as previsões. Você pode então usar esse serviço web em seu site (ou qualquer ambiente de produção que estiver usando). Como você verá no Capítulo 19, implantar modelos TensorFlow no Vertex AI não é muito diferente de implantar modelos Scikit-Learn.

Mas a implantação não é o fim da história. Você também precisa escrever código de monitoramento para verificar o desempenho ao vivo do seu sistema em intervalos regulares e acionar alertas quando ele cair. Ele pode cair muito rapidamente, por exemplo, se um componente quebrar em sua infraestrutura, mas esteja ciente de que ele também pode decair muito lentamente, o que pode passar despercebido por um longo tempo. Isso é bastante comum devido à **degradação do modelo**: se o modelo foi treinado com dados do ano passado, ele pode não estar adaptado aos dados de hoje.

Então, você precisa monitorar o desempenho ao vivo do seu modelo. Mas como você faz isso? Bem, depende. Em alguns casos, o desempenho do modelo pode ser inferido a partir de métricas subsequentes. Por exemplo, se seu modelo faz parte de um sistema de recomendação e sugere produtos que os usuários podem estar interessados, então é fácil monitorar o número de produtos recomendados vendidos a cada dia. Se esse número cair (em comparação com produtos não recomendados), o principal suspeito é o modelo. Isso pode acontecer porque o pipeline de dados está quebrado, ou talvez o modelo precise ser retreinado com dados mais recentes (como discutiremos em breve).

No entanto, você também pode precisar de análise humana para avaliar o desempenho do modelo. Por exemplo, suponha que você treinou um modelo de classificação de imagens (veremos isso no Capítulo 3) para detectar vários defeitos de produtos em uma linha de produção. Como você pode receber um alerta se o desempenho do modelo cair, antes que milhares de produtos defeituosos sejam enviados para seus clientes? Uma solução é enviar para avaliadores humanos uma amostra de todas as imagens que o modelo classificou (especialmente imagens sobre as quais o modelo não tinha tanta certeza). Dependendo da tarefa, os avaliadores podem precisar ser especialistas, ou podem ser não especialistas, como trabalhadores em uma plataforma de crowdsourcing (por exemplo, Amazon Mechanical Turk). Em algumas aplicações, eles poderiam até ser os próprios usuários, respondendo, por exemplo, por meio de pesquisas ou captchas reutilizados.[14]

De qualquer forma, você precisa colocar em prática um sistema de monitoramento (com ou sem avaliadores humanos para avaliar o modelo ao vivo), bem como todos os processos relevantes para definir o que fazer em caso de falhas e como se preparar para elas. Infelizmente, isso pode dar muito trabalho. Na verdade, muitas vezes é muito mais trabalho do que construir e treinar um modelo.

Se os dados continuarem evoluindo, você precisará atualizar seus conjuntos de dados e retreinar seu modelo regularmente. Você provavelmente deve automatizar todo o processo o máximo possível. Aqui estão algumas coisas que você pode automatizar:

- Coletar dados frescos regularmente e rotulá-los (por exemplo, usando avaliadores humanos).
- Escrever um script para treinar o modelo e ajustar os hiperparâmetros automaticamente. Esse script pode ser executado automaticamente, por exemplo, todos os dias ou todas as semanas, dependendo de suas necessidades.
- Escrever outro script que avaliará tanto o novo modelo quanto o modelo anterior no conjunto de teste atualizado e implantará o modelo em produção se o desempenho não tiver diminuído (se tiver, certifique-se de investigar o porquê). O script provavelmente deve testar o desempenho do seu modelo em vários subconjuntos do conjunto de teste, como distritos pobres ou ricos, rurais ou urbanos, etc.

Você também deve garantir que avalie a qualidade dos dados de entrada do modelo. Às vezes, o desempenho degradará um pouco devido a um sinal de baixa qualidade (por exemplo, um sensor com defeito enviando valores aleatórios, ou a saída de outra equipe ficando desatualizada), mas pode levar um tempo até que o desempenho do sistema degrade o suficiente para acionar um alerta. Se você monitorar as entradas do seu modelo, pode detectar isso mais cedo. Por exemplo, você poderia acionar um alerta se mais e mais entradas estiverem faltando uma característica, ou se a média ou o desvio padrão se afastar muito do conjunto de treinamento, ou se uma característica categórica começar a conter novas categorias.

Finalmente, certifique-se de manter backups de todos os modelos que você criar e ter o processo e as ferramentas em vigor para reverter rapidamente para um modelo anterior, caso o novo modelo comece a falhar gravemente por algum motivo. Ter backups também torna possível comparar facilmente novos modelos com os anteriores. Da mesma forma, você deve manter backups de todas as versões de seus conjuntos de dados para que possa reverter para um conjunto de dados anterior se o novo algum dia ficar corrompido (por exemplo, se os dados frescos adicionados a ele estiverem cheios de outliers). Ter backups de seus conjuntos de dados também permite que você avalie qualquer modelo em relação a qualquer conjunto de dados anterior.

Como você pode ver, o aprendizado de máquina envolve bastante infraestrutura. O Capítulo 19 discute alguns aspectos disso, mas é um tópico muito amplo chamado **ML Operations** (MLOps), que merece seu próprio livro. Então, não se surpreenda se seu primeiro projeto de ML exigir muito esforço e tempo para ser construído e implantado em produção. Felizmente, uma vez que toda a infraestrutura estiver em vigor, ir da ideia à produção será muito mais rápido.

---

### Experimente!

Espero que este capítulo tenha dado uma boa ideia de como é um projeto de aprendizado de máquina, além de mostrar algumas das ferramentas que você pode usar para treinar um ótimo sistema. Como você pode ver, muito do trabalho está na etapa de preparação dos dados: construir ferramentas de monitoramento, configurar pipelines de avaliação humana e automatizar o treinamento regular do modelo. Os algoritmos de aprendizado de máquina são importantes, é claro, mas provavelmente é preferível estar confortável com o processo geral e conhecer bem três ou quatro algoritmos do que passar todo o seu tempo explorando algoritmos avançados.

Então, se você ainda não fez isso, agora é um bom momento para pegar um laptop, selecionar um conjunto de dados que lhe interesse e tentar passar por todo o processo de A a Z. Um bom lugar para começar é em um site de competições como o Kaggle: você terá um conjunto de dados para brincar, um objetivo claro e pessoas com quem compartilhar a experiência. Divirta-se!

---

### Exercícios

Os exercícios a seguir são baseados no conjunto de dados de habitação deste capítulo:

1. Tente um regressor de máquina de vetores de suporte (`sklearn.svm.SVR`) com vários hiperparâmetros, como `kernel="linear"` (com vários valores para o hiperparâmetro `C`) ou `kernel="rbf"` (com vários valores para os hiperparâmetros `C` e `gamma`). Observe que as máquinas de vetores de suporte não escalam bem para grandes conjuntos de dados, então você provavelmente deve treinar seu modelo apenas nas primeiras 5.000 instâncias do conjunto de treinamento e usar apenas validação cruzada de 3 folds, ou levará horas. Não se preocupe com o que os hiperparâmetros significam por enquanto; discutiremos isso no Capítulo 5. Como o melhor preditor SVR performa?

2. Tente substituir o `GridSearchCV` por um `RandomizedSearchCV`.

3. Tente adicionar um transformador `SelectFromModel` no pipeline de preparação para selecionar apenas os atributos mais importantes.

4. Tente criar um transformador personalizado que treine um regressor de k-vizinhos mais próximos (`sklearn.neighbors.KNeighborsRegressor`) em seu método `fit()` e produza as previsões do modelo em seu método `transform()`. Em seguida, adicione essa característica ao pipeline de pré-processamento, usando latitude e longitude como entradas para esse transformador. Isso adicionará uma característica ao modelo que corresponde ao preço mediano da casa dos distritos mais próximos.

5. Explore automaticamente algumas opções de preparação usando `GridSearchCV`.

6. Tente implementar a classe `StandardScalerClone` novamente do zero, depois adicione suporte ao método `inverse_transform()`: executar `scaler.inverse_transform(scaler.fit_transform(X))` deve retornar um array muito próximo de `X`. Em seguida, adicione suporte a nomes de características: defina `feature_names_in_` no método `fit()` se a entrada for um DataFrame. Esse atributo deve ser um array NumPy de nomes de colunas. Por fim, implemente o método `get_feature_names_out()`: ele deve ter um argumento opcional `input_features=None`. Se passado, o método deve verificar se seu comprimento corresponde a `n_features_in_` e deve corresponder a `feature_names_in_` se estiver definido; então `input_features` deve ser retornado. Se `input_features` for `None`, o método deve retornar `feature_names_in_` se estiver definido ou `np.array(["x0", "x1", ...])` com comprimento `n_features_in_` caso contrário.

As soluções para esses exercícios estão disponíveis no final do notebook deste capítulo, em https://homl.info/colab3.

---

### Notas de Rodapé

3. Lembre-se de que o operador de transposição transforma um vetor coluna em um vetor linha (e vice-versa).

4. Você também pode precisar verificar restrições legais, como campos privados que nunca devem ser copiados para armazenamentos de dados inseguros.

5. O desvio padrão é geralmente denotado por \(\sigma\) (a letra grega sigma), e é a raiz quadrada da **variância**, que é a média do desvio quadrado da média. Quando uma característica tem uma distribuição **normal** em forma de sino (também chamada de distribuição **Gaussiana**), o que é muito comum, a regra "68-95-99.7" se aplica: cerca de 68% dos valores estão dentro de \(1\sigma\) da média, 95% dentro de \(2\sigma\) e 99.7% dentro de \(3\sigma\).

6. Você frequentemente verá as pessoas definirem a semente aleatória como 42. Esse número não tem nenhuma propriedade especial, além de ser a Resposta para a Vida, o Universo e Tudo Mais.

7. As informações de localização são, na verdade, bastante grosseiras e, como resultado, muitos distritos terão exatamente o mesmo ID, então eles acabarão no mesmo conjunto (teste ou treinamento). Isso introduz algum viés de amostragem infeliz.

8. Se você estiver lendo isso em escala de cinza, pegue uma caneta vermelha e rabisque a maior parte da costa da Bay Area até San Diego (como você poderia esperar). Você também pode adicionar um pouco de amarelo ao redor de Sacramento.

9. Para mais detalhes sobre os princípios de design, veja Lars Buitinck et al., "API Design for Machine Learning Software: Experiences from the Scikit-Learn Project", arXiv preprint arXiv:1309.0238 (2013).

10. Alguns preditores também fornecem métodos para medir a confiança de suas previsões.

11. Quando você ler estas linhas, pode ser possível fazer todos os transformadores retornarem DataFrames do Pandas quando receberem um DataFrame como entrada: Pandas in, Pandas out. Provavelmente haverá uma opção de configuração global para isso: `sklearn.set_config(pandas_in_out=True)`.

12. Veja a documentação do SciPy para mais detalhes.

13. Em resumo, uma API REST (ou RESTful) é uma API baseada em HTTP que segue algumas convenções, como usar verbos HTTP padrão para ler, atualizar, criar ou excluir recursos (GET, POST, PUT e DELETE) e usar JSON para as entradas e saídas.

14. Um captcha é um teste para garantir que um usuário não seja um robô. Esses testes frequentemente foram usados como uma maneira barata de rotular dados de treinamento.

---

