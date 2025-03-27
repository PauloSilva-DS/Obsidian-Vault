---
Atualizado: 2025-03-27  17.15
Criado: 2025-03-27  17.11
tags:
  - estudo
  - python
  - AprendizadoMaquina
Completo: false
---
🔖[[Aprendizado de máquina]]



# Capítulo 2. Projeto de Aprendizado de Máquina de Ponta a Ponta

Neste capítulo, você trabalhará em um projeto exemplo de ponta a ponta, simulando ser um cientista de dados recentemente contratado em uma empresa de imóveis. Este exemplo é fictício; o objetivo é ilustrar as principais etapas de um projeto de aprendizado de máquina, não aprender sobre o mercado imobiliário. Aqui estão as principais etapas que percorreremos:

1. **Entender o panorama geral.**
2. **Obter os dados.**
3. **Explorar e visualizar os dados para obter insights.**
4. **Preparar os dados para algoritmos de aprendizado de máquina.**
5. **Selecionar um modelo e treiná-lo.**
6. **Afinar seu modelo.**
7. **Apresentar sua solução.**
8. **Implementar, monitorar e manter seu sistema.**

## Trabalhando com Dados Reais

Quando você está aprendendo sobre aprendizado de máquina, é melhor experimentar com dados do mundo real, não com conjuntos de dados artificiais. Felizmente, existem milhares de conjuntos de dados abertos para escolher, abrangendo diversos domínios. Aqui estão alguns lugares onde você pode procurar dados:

- **Repositórios de dados abertos populares:**
  - OpenML.org
  - Kaggle.com
  - PapersWithCode.com
  - Repositório de Aprendizado de Máquina da UC Irvine
  - Conjuntos de dados da AWS da Amazon
  - Conjuntos de dados do TensorFlow

- **Portais meta (listam repositórios de dados abertos):**
  - DataPortals.org
  - OpenDataMonitor.eu

- **Outras páginas que listam repositórios de dados abertos populares:**
  - Lista de conjuntos de dados de aprendizado de máquina na Wikipedia
  - Quora.com
  - Subreddit de conjuntos de dados

Neste capítulo, usaremos o conjunto de dados de preços de imóveis na Califórnia do repositório StatLib (veja a Figura 2-1). Este conjunto de dados é baseado em dados do censo da Califórnia de 1990. Não é exatamente recente (uma boa casa na Bay Area ainda era acessível na época), mas tem muitas qualidades para aprendizado, então vamos fingir que são dados recentes. Para fins didáticos, adicionei um atributo categórico e removi alguns recursos.

### Figura 2-1. Preços de imóveis na Califórnia

## Entender o Panorama Geral

Bem-vindo à Corporação de Habitação de Aprendizado de Máquina! Sua primeira tarefa é usar dados do censo da Califórnia para construir um modelo de preços de imóveis no estado. Esses dados incluem métricas como população, renda média e preço médio de imóveis para cada grupo de blocos na Califórnia. Grupos de blocos são a menor unidade geográfica para a qual o US Census Bureau publica dados amostrais (um grupo de blocos normalmente tem uma população de 600 a 3.000 pessoas). Vou chamá-los de "distritos" para simplificar.

Seu modelo deve aprender com esses dados e ser capaz de prever o preço médio de imóveis em qualquer distrito, dadas todas as outras métricas.

**DICA**

Como você é um cientista de dados organizado, a primeira coisa que deve fazer é pegar sua lista de verificação de projetos de aprendizado de máquina. Você pode começar com a do Apêndice A; ela deve funcionar razoavelmente bem para a maioria dos projetos de aprendizado de máquina, mas certifique-se de adaptá-la às suas necessidades. Neste capítulo, passaremos por muitos itens da lista de verificação, mas também pularemos alguns, seja porque são autoexplicativos ou porque serão discutidos em capítulos posteriores.

### Enquadrar o Problema

A primeira pergunta a fazer ao seu chefe é qual exatamente é o objetivo de negócios. Construir um modelo provavelmente não é o objetivo final. Como a empresa espera usar e se beneficiar desse modelo? Saber o objetivo é importante porque determinará como você enquadra o problema, quais algoritmos selecionará, qual medida de desempenho usará para avaliar seu modelo e quanto esforço gastará em ajustá-lo.

Seu chefe responde que a saída do seu modelo (uma previsão do preço médio de imóveis de um distrito) será alimentada em outro sistema de aprendizado de máquina (veja a Figura 2-2), junto com muitos outros sinais. Esse sistema downstream determinará se vale a pena investir em uma determinada área. Acertar isso é crítico, pois afeta diretamente a receita.

A próxima pergunta a fazer ao seu chefe é como é a solução atual (se houver). A situação atual geralmente fornece uma referência de desempenho, bem como insights sobre como resolver o problema. Seu chefe responde que os preços de imóveis dos distritos atualmente são estimados manualmente por especialistas: uma equipe coleta informações atualizadas sobre um distrito e, quando não conseguem obter o preço médio de imóveis, estimam-no usando regras complexas.

### Figura 2-2. Um pipeline de aprendizado de máquina para investimentos em imóveis

Isso é caro e demorado, e suas estimativas não são ótimas; nos casos em que conseguem descobrir o preço médio real de imóveis, frequentemente percebem que suas estimativas estavam erradas em mais de 30%. É por isso que a empresa acha que seria útil treinar um modelo para prever o preço médio de imóveis de um distrito, dados outros dados sobre esse distrito. Os dados do censo parecem um ótimo conjunto de dados para explorar para esse propósito, pois incluem os preços médios de imóveis de milhares de distritos, além de outros dados.

**PIPELINES**

Uma sequência de componentes de processamento de dados é chamada de pipeline de dados. Pipelines são muito comuns em sistemas de aprendizado de máquina, pois há muitos dados para manipular e muitas transformações de dados para aplicar.

Os componentes geralmente são executados de forma assíncrona. Cada componente puxa uma grande quantidade de dados, processa-os e gera o resultado em outro armazenamento de dados. Então, algum tempo depois, o próximo componente no pipeline puxa esses dados e gera sua própria saída. Cada componente é bastante autônomo: a interface entre os componentes é simplesmente o armazenamento de dados. Isso torna o sistema simples de entender (com a ajuda de um gráfico de fluxo de dados), e diferentes equipes podem se concentrar em diferentes componentes. Além disso, se um componente falhar, os componentes downstream geralmente podem continuar funcionando normalmente (pelo menos por um tempo) usando apenas a última saída do componente quebrado. Isso torna a arquitetura bastante robusta.

Por outro lado, um componente quebrado pode passar despercebido por algum tempo se o monitoramento adequado não for implementado. Os dados ficam desatualizados e o desempenho geral do sistema cai.

Com todas essas informações, você agora está pronto para começar a projetar seu sistema. Primeiro, determine que tipo de supervisão de treinamento o modelo precisará: é uma tarefa de aprendizado supervisionado, não supervisionado, semissupervisionado, autor supervisionado ou por reforço? E é uma tarefa de classificação, regressão ou algo diferente? Você deve usar técnicas de aprendizado em lote ou online? Antes de continuar, pause e tente responder a essas perguntas por si mesmo.

Você encontrou as respostas? Vamos ver. Isso é claramente uma tarefa típica de aprendizado supervisionado, pois o modelo pode ser treinado com exemplos rotulados (cada instância vem com a saída esperada, ou seja, o preço médio de imóveis do distrito). É uma tarefa típica de regressão, pois o modelo será solicitado a prever um valor. Mais especificamente, é um problema de regressão múltipla, pois o sistema usará vários recursos para fazer uma previsão (população do distrito, renda média, etc.). Também é um problema de regressão univariada, pois estamos tentando prever apenas um único valor para cada distrito. Se estivéssemos tentando prever vários valores por distrito, seria um problema de regressão multivariada. Finalmente, não há um fluxo contínuo de dados entrando no sistema, não há necessidade particular de se ajustar rapidamente a mudanças nos dados, e os dados são pequenos o suficiente para caber na memória, então o aprendizado em lote simples deve funcionar bem.

**DICA**

Se os dados fossem enormes, você poderia dividir seu trabalho de aprendizado em lote em vários servidores (usando a técnica MapReduce) ou usar uma técnica de aprendizado online.

### Selecionar uma Medida de Desempenho

Seu próximo passo é selecionar uma medida de desempenho. Uma medida típica para problemas de regressão é o erro quadrático médio (RMSE). Ele dá uma ideia de quanto erro o sistema normalmente comete em suas previsões, com um peso maior dado a erros grandes. A Equação 2-1 mostra a fórmula matemática para calcular o RMSE.

**Equação 2-1. Erro quadrático médio (RMSE)**

RMSE(X,h)= √(1/m ∑(h(x(i))−y(i))²)

**NOTAÇÕES**

Esta equação introduz várias notações muito comuns em aprendizado de máquina que usarei ao longo deste livro:

- m é o número de instâncias no conjunto de dados que você está medindo o RMSE.
  - Por exemplo, se você está avaliando o RMSE em um conjunto de validação de 2.000 distritos, então m = 2.000.
- x(i) é um vetor de todos os valores dos recursos (excluindo o rótulo) da i-ésima instância no conjunto de dados, e y(i) é seu rótulo (o valor de saída desejado para essa instância).
  - Por exemplo, se o primeiro distrito no conjunto de dados está localizado na longitude –118,29°, latitude 33,91°, tem 1.416 habitantes com uma renda média de $38.372, e o valor médio da casa é $156.400 (ignorando outros recursos por enquanto), então:
    - x(1) = (–118,29, 33,91, 1.416, 38.372)
    - y(1) = 156.400
- X é uma matriz contendo todos os valores dos recursos (excluindo rótulos) de todas as instâncias no conjunto de dados. Há uma linha por instância, e a i-ésima linha é igual à transposta de x(i), denotada (x(i))⊺.
  - Por exemplo, se o primeiro distrito for como acabamos de descrever, então a matriz X se parece com isso:
    - X = [ (x(1))⊺, (x(2))⊺, ..., (x(2000))⊺ ]⊺
- h é a função de previsão do seu sistema, também chamada de hipótese. Quando seu sistema recebe o vetor de recursos x(i) de uma instância, ele gera um valor previsto ŷ = h(x(i)) para essa instância (ŷ é pronunciado "y-chapéu").
  - Por exemplo, se seu sistema prevê que o preço médio de imóveis no primeiro distrito é $158.400, então ŷ = h(x(1)) = 158.400. O erro de previsão para este distrito é ŷ – y(1) = 2.000.
- RMSE(X,h) é a função de custo medida no conjunto de exemplos usando sua hipótese h.

Usamos fonte itálica em minúsculas para valores escalares (como m ou y(i)) e nomes de funções (como h), fonte em negrito em minúsculas para vetores (como x(i)) e fonte em negrito em maiúsculas para matrizes (como X).

Embora o RMSE seja geralmente a medida de desempenho preferida para tarefas de regressão, em alguns contextos você pode preferir usar outra função. Por exemplo, se houver muitos distritos outliers. Nesse caso, você pode considerar usar o erro absoluto médio (MAE, também chamado de desvio absoluto médio), mostrado na Equação 2-2:

**Equação 2-2. Erro absoluto médio (MAE)**

MAE(X,h)= (1/m) ∑ |h(x(i))−y(i)|

Tanto o RMSE quanto o MAE são formas de medir a distância entre dois vetores: o vetor de previsões e o vetor de valores alvo. Várias medidas de distância, ou normas, são possíveis:

- Calcular a raiz de uma soma de quadrados (RMSE) corresponde à norma euclidiana: essa é a noção de distância com a qual todos estamos familiarizados. Também é chamada de norma ℓ₂, denotada ∥ · ∥₂ (ou apenas ∥ · ∥).
- Calcular a soma de valores absolutos (MAE) corresponde à norma ℓ₁, denotada ∥ · ∥₁. Isso às vezes é chamado de norma de Manhattan porque mede a distância entre dois pontos em uma cidade se você só puder viajar ao longo de blocos de cidade ortogonais.
- Mais geralmente, a norma ℓₖ de um vetor v contendo n elementos é definida como ∥v∥ₖ = (|v₁|ᵏ + |v₂|ᵏ + ... + |vₙ|ᵏ)^(1/k). ℓ₀ dá o número de elementos não nulos no vetor, e ℓ∞ dá o valor absoluto máximo no vetor.

Quanto maior o índice da norma, mais ela se concentra em valores grandes e negligencia valores pequenos. É por isso que o RMSE é mais sensível a outliers que o MAE. Mas quando outliers são exponencialmente raros (como em uma curva em forma de sino), o RMSE funciona muito bem e geralmente é preferido.

### Verificar as Suposições

Por fim, é uma boa prática listar e verificar as suposições feitas até agora (por você ou por outros); isso pode ajudá-lo a detectar problemas sérios desde o início. Por exemplo, os preços dos distritos que seu sistema gera serão alimentados em um sistema de aprendizado de máquina downstream, e você assume que esses preços serão usados como estão. Mas e se o sistema downstream converter os preços em categorias (por exemplo, "barato", "médio" ou "caro") e então usar essas categorias em vez dos preços em si? Nesse caso, obter o preço perfeitamente correto não é importante; seu sistema só precisa acertar a categoria. Se for esse o caso, então o problema deveria ter sido enquadrado como uma tarefa de classificação, não de regressão. Você não quer descobrir isso depois de trabalhar em um sistema de regressão por meses.

Felizmente, depois de conversar com a equipe responsável pelo sistema downstream, você está confiante de que eles realmente precisam dos preços reais, não apenas de categorias. Ótimo! Você está pronto, as luzes estão verdes e pode começar a codificar agora!

## Obter os Dados

É hora de sujar as mãos. Não hesite em pegar seu laptop e percorrer os exemplos de código. Como mencionei no prefácio, todos os exemplos de código neste livro são de código aberto e estão disponíveis online como notebooks Jupyter, que são documentos interativos contendo texto, imagens e trechos de código executáveis (Python no nosso caso). Neste livro, assumirei que você está executando esses notebooks no Google Colab, um serviço gratuito que permite executar qualquer notebook Jupyter diretamente online, sem precisar instalar nada em sua máquina. Se você quiser usar outra plataforma online (por exemplo, Kaggle) ou instalar tudo localmente em sua própria máquina, consulte as instruções na página do GitHub do livro.

### Executando os Exemplos de Código Usando o Google Colab

Primeiro, abra um navegador da web e visite https://homl.info/colab3: isso o levará ao Google Colab e exibirá a lista de notebooks Jupyter para este livro (veja a Figura 2-3). Você encontrará um notebook por capítulo, além de alguns notebooks extras e tutoriais para NumPy, Matplotlib, Pandas, álgebra linear e cálculo diferencial. Por exemplo, se você clicar em 02_end_to_end_machine_learning_project.ipynb, o notebook do Capítulo 2 será aberto no Google Colab (veja a Figura 2-4).

Um notebook Jupyter é composto por uma lista de células. Cada célula contém código executável ou texto. Tente clicar duas vezes na primeira célula de texto (que contém a frase "Bem-vindo à Corporação de Habitação de Aprendizado de Máquina!"). Isso abrirá a célula para edição. Observe que os notebooks Jupyter usam sintaxe Markdown para formatação (por exemplo, negrito, itálico, # Título, [url](texto do link), etc.). Tente modificar este texto e pressione Shift-Enter para ver o resultado.

### Figura 2-3. Lista de notebooks no Google Colab

### Figura 2-4. Seu notebook no Google Colab

Em seguida, crie uma nova célula de código selecionando Inserir → "Célula de código" no menu. Alternativamente, você pode clicar no botão + Código na barra de ferramentas ou passar o mouse sobre a parte inferior de uma célula até ver + Código e + Texto aparecerem, então clique em + Código. Na nova célula de código, digite algum código Python, como print("Hello World"), e pressione Shift-Enter para executar este código (ou clique no botão ▷ no lado esquerdo da célula).

Se você não estiver logado na sua conta do Google, será solicitado que faça login agora (se você ainda não tiver uma conta do Google, precisará criar uma). Depois de logado, ao tentar executar o código, você verá um aviso de segurança informando que este notebook não foi criado pelo Google. Uma pessoa mal-intencionada poderia criar um notebook que tenta enganá-lo para inserir suas credenciais do Google e acessar seus dados pessoais, então, antes de executar um notebook, sempre certifique-se de confiar em seu autor (ou verifique o que cada célula de código fará antes de executá-la). Supondo que você confie em mim (ou planeje verificar cada célula de código), você pode clicar em "Executar mesmo assim".

O Colab então alocará um novo runtime para você: esta é uma máquina virtual gratuita localizada nos servidores do Google que contém várias ferramentas e bibliotecas Python, incluindo tudo o que você precisará para a maioria dos capítulos (em alguns capítulos, você precisará executar um comando para instalar bibliotecas adicionais). Isso levará alguns segundos. Em seguida, o Colab se conectará automaticamente a este runtime e o usará para executar sua nova célula de código. Importante: o código é executado no runtime, não na sua máquina. A saída do código será exibida abaixo da célula. Parabéns, você executou algum código Python no Colab!

**DICA**

Para inserir uma nova célula de código, você também pode digitar Ctrl-M (ou Cmd-M no macOS) seguido de A (para inserir acima da célula ativa) ou B (para inserir abaixo). Existem muitos outros atalhos de teclado disponíveis: você pode visualizá-los e editá-los digitando Ctrl-M (ou Cmd-M) e depois H. Se você optar por executar os notebooks no Kaggle ou em sua própria máquina usando JupyterLab ou uma IDE como Visual Studio Code com a extensão Jupyter, verá algumas pequenas diferenças—os runtimes são chamados de kernels, a interface do usuário e os atalhos de teclado são ligeiramente diferentes, etc.—mas alternar entre um ambiente Jupyter e outro não é muito difícil.

### Salvando Suas Alterações de Código e Seus Dados

Você pode fazer alterações em um notebook do Colab, e elas persistirão enquanto você mantiver a aba do navegador aberta. Mas assim que você fechá-la, as alterações serão perdidas. Para evitar isso, certifique-se de salvar uma cópia do notebook no seu Google Drive selecionando Arquivo → "Salvar uma cópia no Drive".

Alternativamente, você pode baixar o notebook para o seu computador selecionando Arquivo → Download → "Download .ipynb". Depois, você pode visitar https://colab.research.google.com e abrir o notebook novamente (do Google Drive ou fazendo upload do seu computador).

**AVISO**

O Google Colab é destinado apenas para uso interativo: você pode brincar nos notebooks e ajustar o código como quiser, mas não pode deixar os notebooks rodando sem supervisão por um longo período de tempo, ou o runtime será desligado e todos os seus dados serão perdidos.

Se o notebook gerar dados que são importantes para você, certifique-se de baixá-los antes que o runtime seja desligado. Para fazer isso, clique no ícone Arquivos (veja a etapa 1 na Figura 2-5), encontre o arquivo que deseja baixar, clique nos pontos verticais ao lado dele (etapa 2) e clique em Download (etapa 3). Alternativamente, você pode montar seu Google Drive no runtime, permitindo que o notebook leia e escreva arquivos diretamente no Google Drive como se fosse um diretório local. Para isso, clique no ícone Arquivos (etapa 1), depois no ícone do Google Drive (circulado na Figura 2-5) e siga as instruções na tela.

### Figura 2-5. Baixando um arquivo de um runtime do Google Colab (etapas 1 a 3) ou montando seu Google Drive (ícone circulado)

Por padrão, seu Google Drive será montado em /content/drive/MyDrive. Se você quiser fazer backup de um arquivo de dados, simplesmente copie-o para este diretório executando !cp /content/my_great_model /content/drive/MyDrive.

Qualquer comando que começa com um ponto de exclamação (!) é tratado como um comando do shell, não como código Python: cp é o comando do shell Linux para copiar um arquivo de um caminho para outro. Observe que os runtimes do Colab rodam no Linux (especificamente, Ubuntu).

### O Poder e o Perigo da Interatividade

Os notebooks Jupyter são interativos, e isso é ótimo: você pode executar cada célula uma por uma, parar a qualquer momento, inserir uma célula, brincar com o código, voltar e executar a mesma célula novamente, etc., e eu o incentivo fortemente a fazer isso. Se você apenas executar as células uma por uma sem nunca brincar com elas, não aprenderá tão rápido. No entanto, essa flexibilidade tem um preço: é muito fácil executar células na ordem errada ou esquecer de executar uma célula. Se isso acontecer, as células de código subsequentes provavelmente falharão. Por exemplo, a primeira célula de código em cada notebook contém código de configuração (como imports), então certifique-se de executá-la primeiro, ou nada funcionará.

**DICA**

Se você encontrar um erro estranho, tente reiniciar o runtime (selecionando Runtime → "Reiniciar runtime" no menu) e então execute todas as células novamente desde o início do notebook. Isso geralmente resolve o problema. Caso contrário, é provável que uma das alterações que você fez tenha quebrado o notebook: basta reverter para o notebook original e tentar novamente. Se ainda falhar, por favor, abra um problema no GitHub.

### Código do Livro Versus Código do Notebook

Você pode às vezes notar algumas pequenas diferenças entre o código neste livro e o código nos notebooks. Isso pode acontecer por várias razões:

- Uma biblioteca pode ter mudado um pouco no momento em que você ler estas linhas, ou talvez, apesar dos meus melhores esforços, eu tenha cometido um erro no livro. Infelizmente, não posso magicamente corrigir o código na sua cópia deste livro (a menos que você esteja lendo uma cópia eletrônica e possa baixar a versão mais recente), mas posso corrigir os notebooks. Então, se você encontrar um erro depois de copiar código deste livro, procure o código corrigido nos notebooks: me esforçarei para mantê-los livres de erros e atualizados com as versões mais recentes das bibliotecas.
- Os notebooks contêm algum código extra para embelezar as figuras (adicionar rótulos, definir tamanhos de fonte, etc.) e salvá-las em alta resolução para este livro. Você pode ignorar com segurança esse código extra se quiser.
- Otimizei o código para legibilidade e simplicidade: fiz o mais linear e plano possível, definindo muito poucas funções ou classes. O objetivo é garantir que o código que você está executando geralmente esteja bem na sua frente, e não aninhado dentro de várias camadas de abstrações que você teria que procurar. Isso também facilita a brincadeira com o código. Para simplificar, há tratamento de erros limitado, e coloquei alguns dos imports menos comuns exatamente onde são necessários (em vez de colocá-los no topo do arquivo, como é recomendado pelo guia de estilo PEP 8 do Python). Dito isso, seu código de produção não será muito diferente: apenas um pouco mais modular e com testes e tratamento de erros adicionais.

OK! Quando você estiver confortável com o Colab, estará pronto para baixar os dados.

## Baixar os Dados

Em ambientes típicos, seus dados estariam disponíveis em um banco de dados relacional ou em algum outro armazenamento de dados comum, e espalhados por várias tabelas/documentos/arquivos. Para acessá-los, você primeiro precisaria obter suas credenciais e autorizações de acesso e se familiarizar com o esquema dos dados. Neste projeto, no entanto, as coisas são muito mais simples: você apenas baixará um único arquivo compactado, housing.tgz, que contém um arquivo de valores separados por vírgulas (CSV) chamado housing.csv com todos os dados.

Em vez de baixar e descompactar os dados manualmente, geralmente é preferível escrever uma função que faça isso por você. Isso é útil especialmente se os dados mudarem regularmente: você pode escrever um pequeno script que usa a função para buscar os dados mais recentes (ou pode configurar um trabalho agendado para fazer isso automaticamente em intervalos regulares). Automatizar o processo de busca dos dados também é útil se você precisar instalar o conjunto de dados em várias máquinas.

Aqui está a função para buscar e carregar os dados:

```python
from pathlib import Path
import pandas as pd
import tarfile
import urllib.request

def load_housing_data():
    tarball_path = Path("datasets/housing.tgz")
    if not tarball_path.is_file():
        Path("datasets").mkdir(parents=True, exist_ok=True)
        url = "https://github.com/ageron/data/raw/main/housing.tgz"
        urllib.request.urlretrieve(url, tarball_path)
    with tarfile.open(tarball_path) as housing_tarball:
        housing_tarball.extractall(path="datasets")
    return pd.read_csv(Path("datasets/housing/housing.csv"))

housing = load_housing_data()
```

Quando load_housing_data() é chamada, ela procura pelo arquivo datasets/housing.tgz. Se não encontrá-lo, cria o diretório datasets dentro do diretório atual (que é /content por padrão, no Colab), baixa o arquivo housing.tgz do repositório ageron/data no GitHub e extrai seu conteúdo no diretório datasets; isso cria o diretório datasets/housing com o arquivo housing.csv dentro. Por fim, a função carrega este arquivo CSV em um objeto DataFrame do Pandas contendo todos os dados e o retorna.

### Dê uma Olhada Rápida na Estrutura dos Dados

Você começa olhando as cinco primeiras linhas de dados usando o método head() do DataFrame (veja a Figura 2-6).

### Figura 2-6. As cinco primeiras linhas no conjunto de dados

Cada linha representa um distrito. Há 10 atributos (eles não são todos mostrados na captura de tela): longitude, latitude, housing_median_age, total_rooms, total_bedrooms, population, households, median_income, median_house_value e ocean_proximity.

O método info() é útil para obter uma descrição rápida dos dados, em particular o número total de linhas, o tipo de cada atributo e o número de valores não nulos:

```python
>>> housing.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 20640 entries, 0 to 20639
Data columns (total 10 columns):
 #   Column              Non-Null Count  Dtype  
---  ------              --------------  -----  
 0   longitude           20640 non-null  float64
 1   latitude            20640 non-null  float64
 2   housing_median_age  20640 non-null  float64
 3   total_rooms         20640 non-null  float64
 4   total_bedrooms      20433 non-null  float64
 5   population          20640 non-null  float64
 6   households          20640 non-null  float64
 7   median_income       20640 non-null  float64
 8   median_house_value  20640 non-null  float64
 9   ocean_proximity     20640 non-null  object 
dtypes: float64(9), object(1)
memory usage: 1.6+ MB
```

**OBSERVAÇÃO**

Neste livro, quando um exemplo de código contém uma mistura de código e saídas, como é o caso aqui, ele é formatado como no interpretador Python, para melhor legibilidade: as linhas de código são prefixadas com >>> (ou ... para blocos recuados), e as saídas não têm prefixo.

Há 20.640 instâncias no conjunto de dados, o que significa que é bastante pequeno para os padrões de aprendizado de máquina, mas é perfeito para começar. Você nota que o atributo total_bedrooms tem apenas 20.433 valores não nulos, o que significa que 207 distritos estão faltando este recurso. Você precisará lidar com isso mais tarde.

Todos os atributos são numéricos, exceto ocean_proximity. Seu tipo é object, então pode conter qualquer tipo de objeto Python. Mas como você carregou esses dados de um arquivo CSV, sabe que deve ser um atributo de texto. Quando você olhou as cinco primeiras linhas, provavelmente notou que os valores na coluna ocean_proximity eram repetitivos, o que significa que provavelmente é um atributo categórico. Você pode descobrir quais categorias existem e quantos distritos pertencem a cada categoria usando o método value_counts():

```python
>>> housing["ocean_proximity"].value_counts()
<1H OCEAN     9136
INLAND        6551
NEAR OCEAN    2658
NEAR BAY      2290
ISLAND           5
Name: ocean_proximity, dtype: int64
```

Vamos olhar para os outros campos. O método describe() mostra um resumo dos atributos numéricos (Figura 2-7).

### Figura 2-7. Resumo de cada atributo numérico

As linhas count, mean, min e max são autoexplicativas. Observe que os valores nulos são ignorados (então, por exemplo, a contagem de total_bedrooms é 20.433, não 20.640). A linha std mostra o desvio padrão, que mede o quão dispersos os valores estão. As linhas 25%, 50% e 75% mostram os percentis correspondentes: um percentil indica o valor abaixo do qual uma determinada porcentagem de observações em um grupo de observações cai. Por exemplo, 25% dos distritos têm um housing_median_age menor que 18, enquanto 50% são menores que 29 e 75% são menores que 37. Esses são frequentemente chamados de percentil 25 (ou primeiro quartil), mediana e percentil 75 (ou terceiro quartil).

Outra maneira rápida de ter uma noção do tipo de dados com que você está lidando é plotar um histograma para cada atributo numérico. Um histograma mostra o número de instâncias (no eixo vertical) que têm um determinado intervalo de valores (no eixo horizontal). Você pode plotar um atributo por vez ou chamar o método hist() em todo o conjunto de dados (como mostrado no seguinte exemplo de código), e ele plotará um histograma para cada atributo numérico (veja a Figura 2-8):

```python
import matplotlib.pyplot as plt

housing.hist(bins=50, figsize=(12, 8))
plt.show()
```

### Figura 2-8. Um histograma para cada atributo numérico

Olhando para esses histogramas, você nota algumas coisas:

1. Primeiro, o atributo median_income não parece estar expresso em dólares americanos (USD). Depois de verificar com a equipe que coletou os dados, você é informado de que os dados foram escalados e limitados em 15 (na verdade, 15,0001) para rendas medianas mais altas e em 0,5 (na verdade, 0,4999) para rendas medianas mais baixas. Os números representam aproximadamente dezenas de milhares de dólares (por exemplo, 3 significa cerca de $30.000). Trabalhar com atributos pré-processados é comum em aprendizado de máquina, e não é necessariamente um problema, mas você deve tentar entender como os dados foram computados.

2. O housing_median_age e o median_house_value também foram limitados. O último pode ser um problema sério, pois é seu atributo alvo (seus rótulos). Seus algoritmos de aprendizado de máquina podem aprender que os preços nunca ultrapassam esse limite. Você precisa verificar com sua equipe de clientes (a equipe que usará a saída do seu sistema) para ver se isso é um problema ou não. Se eles disserem que precisam de previsões precisas mesmo além de $500.000, então você tem duas opções:
   - Coletar rótulos adequados para os distritos cujos rótulos foram limitados.
   - Remover esses distritos do conjunto de treinamento (e também do conjunto de teste, pois seu sistema não deve ser avaliado de forma inadequada se prever valores além de $500.000).

3. Esses atributos têm escalas muito diferentes. Discutiremos isso mais tarde neste capítulo, quando explorarmos o dimensionamento de recursos.

4. Finalmente, muitos histogramas são assimétricos à direita: eles se estendem muito mais à direita da mediana do que à esquerda. Isso pode dificultar a detecção de padrões por alguns algoritmos de aprendizado de máquina. Mais tarde, você tentará transformar esses atributos para ter distribuições mais simétricas e em forma de sino.

Agora você deve ter uma compreensão melhor do tipo de dados com que está lidando.

**AVISO**

Espere! Antes de olhar para os dados mais a fundo, você precisa criar um conjunto de teste, colocá-lo de lado e nunca olhar para ele.

## Criar um Conjunto de Teste

Pode parecer estranho reservar voluntariamente parte dos dados nesta fase. Afinal, você só deu uma olhada rápida nos dados, e certamente deve aprender muito mais sobre eles antes de decidir quais algoritmos usar, certo? Isso é verdade, mas seu cérebro é um sistema incrível de detecção de padrões, o que também significa que é altamente propenso a overfitting: se você olhar para o conjunto de teste, pode se deparar com algum padrão aparentemente interessante nos dados de teste que o leve a selecionar um tipo particular de modelo de aprendizado de máquina. Quando você estimar o erro de generalização usando o conjunto de teste, sua estimativa será muito otimista, e você lançará um sistema que não terá o desempenho esperado. Isso é chamado de viés de espiada de dados.

Criar um conjunto de teste é teoricamente simples; escolha algumas instâncias aleatoriamente, tipicamente 20% do conjunto de dados (ou menos se seu conjunto de dados for muito grande), e reserve-as:

```python
import numpy as np

def shuffle_and_split_data(data, test_ratio):
    shuffled_indices = np.random.permutation(len(data))
    test_set_size = int(len(data) * test_ratio)
    test_indices = shuffled_indices[:test_set_size]
    train_indices = shuffled_indices[test_set_size:]
    return data.iloc[train_indices], data.iloc[test_indices]
```

Você pode então usar esta função assim:

```python
>>> train_set, test_set = shuffle_and_split_data(housing, 0.2)
>>> len(train_set)
16512
>>> len(test_set)
4128
```

Bem, isso funciona, mas não é perfeito: se você executar o programa novamente, ele gerará um conjunto de teste diferente! Com o tempo, você (ou seus algoritmos de aprendizado de máquina) acabará vendo todo o conjunto de dados, o que é exatamente o que você quer evitar.

Uma solução é salvar o conjunto de teste na primeira execução e carregá-lo em execuções subsequentes. Outra opção é definir a semente do gerador de números aleatórios (por exemplo, com np.random.seed(42)) antes de chamar np.random.permutation(), para que ele sempre gere os mesmos índices embaralhados.

No entanto, ambas as soluções quebrarão na próxima vez que você buscar um conjunto de dados atualizado. Para ter uma divisão estável entre treino/teste, mesmo após atualizar o conjunto de dados, uma solução comum é usar o identificador de cada instância para decidir se ela deve ir para o conjunto de teste (assumindo que as instâncias tenham identificadores únicos e imutáveis). Por exemplo, você pode calcular um hash do identificador de cada instância e colocar essa instância no conjunto de teste se o hash for menor ou igual a 20% do valor máximo de hash. Isso garante que o conjunto de teste permanecerá consistente em várias execuções, mesmo se você atualizar o conjunto de dados. O novo conjunto de teste conterá 20% das novas instâncias, mas não conterá nenhuma instância que estava anteriormente no conjunto de treinamento.

Aqui está uma possível implementação:

```python
from zlib import crc32

def is_id_in_test_set(identifier, test_ratio):
    return crc32(np.int64(identifier)) < test_ratio * 2 ** 32

def split_data_with_id_hash(data, test_ratio, id_column):
    ids = data[id_column]
    in_test_set = ids.apply(lambda id_: is_id_in_test_set(id_, test_ratio))
    return data.loc[~in_test_set], data.loc[in_test_set]
```

Infelizmente, o conjunto de dados de habitação não tem uma coluna de identificador. A solução mais simples é usar o índice da linha como ID:

```python
housing_with_id = housing.reset_index()  # adds an `index` column
train_set, test_set = split_data_with_id_hash(housing_with_id, 0.2, "index")
```

Se você usar o índice da linha como um identificador único, precisa garantir que novos dados sejam anexados ao final do conjunto de dados e que nenhuma linha seja excluída. Se isso não for possível, então você pode tentar usar os recursos mais estáveis para construir um identificador único. Por exemplo, a latitude e longitude de um distrito são garantidas como estáveis por alguns milhões de anos, então você pode combiná-las em um ID assim:

```python
housing_with_id["id"] = housing["longitude"] * 1000 + housing["latitude"]
train_set, test_set = split_data_with_id_hash(housing_with_id, 0.2, "id")
```

O Scikit-Learn fornece algumas funções para dividir conjuntos de dados em vários subconjuntos de várias maneiras. A função mais simples é train_test_split(), que faz basicamente a mesma coisa que a função shuffle_and_split_data() que definimos anteriormente, com alguns recursos adicionais. Primeiro, há um parâmetro random_state que permite definir a semente do gerador aleatório. Segundo, você pode passar vários conjuntos de dados com um número idêntico de linhas, e ele os dividirá nos mesmos índices (isso é muito útil, por exemplo, se você tiver um DataFrame separado para rótulos):

```python
from sklearn.model_selection import train_test_split

train_set, test_set = train_test_split(housing, test_size=0.2, random_state=42)
```

Até agora consideramos métodos de amostragem puramente aleatórios. Isso geralmente é bom se seu conjunto de dados for grande o suficiente (especialmente em relação ao número de atributos), mas se não for, você corre o risco de introduzir um viés de amostragem significativo. Quando funcionários de uma empresa de pesquisa decidem ligar para 1.000 pessoas para fazer algumas perguntas, eles não escolhem apenas 1.000 pessoas aleatoriamente em uma lista telefônica. Eles tentam garantir que essas 1.000 pessoas sejam representativas de toda a população, em relação às perguntas que querem fazer. Por exemplo, a população dos EUA é 51,1% feminina e 48,9% masculina, então uma pesquisa bem conduzida nos EUA tentaria manter essa proporção na amostra: 511 mulheres e 489 homens (pelo menos se parecer possível que as respostas variem entre os gêneros). Isso é chamado de amostragem estratificada: a população é dividida em subgrupos homogêneos chamados estratos, e o número certo de instâncias é amostrado de cada estrato para garantir que o conjunto de teste seja representativo da população geral. Se os pesquisadores usassem amostragem puramente aleatória, haveria cerca de 10,7% de chance de amostrar um conjunto de teste enviesado com menos de 48,5% de participantes femininas ou mais de 53,5%. De qualquer forma, os resultados da pesquisa provavelmente seriam bastante tendenciosos.

Suponha que você tenha conversado com alguns especialistas que disseram que a renda mediana é um atributo muito importante para prever os preços médios de imóveis. Você pode querer garantir que o conjunto de teste seja representativo das várias categorias de renda em todo o conjunto de dados. Como a renda mediana é um atributo numérico contínuo, primeiro você precisa criar uma categoria de renda. Vamos dar uma olhada mais de perto no histograma da renda mediana (de volta na Figura 2-8): a maioria dos valores de renda mediana está agrupada em torno de 1,5 a 6 (ou seja, $15.000–$60.000), mas algumas rendas medianas vão muito além de 6. É importante ter um número suficiente de instâncias em seu conjunto de dados para cada estrato, ou a estimativa da importância de um estrato pode ser tendenciosa. Isso significa que você não deve ter muitos estratos, e cada estrato deve ser grande o suficiente. O código a seguir usa a função pd.cut() para criar um atributo de categoria de renda com cinco categorias (rotuladas de 1 a 5); a categoria 1 varia de 0 a 1,5 (ou seja, menos de $15.000), a categoria 2 de 1,5 a 3, e assim por diante:

```python
housing["income_cat"] = pd.cut(housing["median_income"],
                               bins=[0., 1.5, 3.0, 4.5, 6., np.inf],
                               labels=[1, 2, 3, 4, 5])
```

Essas categorias de renda são representadas na Figura 2-9:

```python
housing["income_cat"].value_counts().sort_index().plot.bar(rot=0, grid=True)
plt.xlabel("Income category")
plt.ylabel("Number of districts")
plt.show()
```

Agora você está pronto para fazer amostragem estratificada com base na categoria de renda. O Scikit-Learn fornece algumas classes de divisão no pacote sklearn.model_selection que implementam várias estratégias para dividir seu conjunto de dados em um conjunto de treinamento e um conjunto de teste. Cada divisor tem um método split() que retorna um iterador sobre diferentes divisões de treino/teste dos mesmos dados.

### Figura 2-9. Histograma de categorias de renda

Para ser preciso, o método split() gera os índices de treinamento e teste, não os dados em si. Ter várias divisões pode ser útil se você quiser estimar melhor o desempenho do seu modelo, como veremos quando discutirmos validação cruzada mais adiante neste capítulo. Por exemplo, o código a seguir gera 10 divisões estratificadas diferentes do mesmo conjunto de dados:

```python
from sklearn.model_selection import StratifiedShuffleSplit

splitter = StratifiedShuffleSplit(n_splits=10, test_size=0.2, random_state=42)
strat_splits = []
for train_index, test_index in splitter.split(housing, housing["income_cat"]):
    strat_train_set_n = housing.iloc[train_index]
    strat_test_set_n = housing.iloc[test_index]
    strat_splits.append([strat_train_set_n, strat_test_set_n])
```

Por enquanto, você pode apenas usar a primeira divisão:

```python
strat_train_set, strat_test_set = strat_splits[0]
```

Ou, como a amostragem estratificada é bastante comum, há uma maneira mais curta de obter uma única divisão usando a função train_test_split() com o argumento stratify:

```python
strat_train_set, strat_test_set = train_test_split(
    housing, test_size=0.2, stratify=housing["income_cat"], random_state=42)
```

Vamos ver se isso funcionou como esperado. Você pode começar olhando as proporções das categorias de renda no conjunto de teste:

```python
>>> strat_test_set["income_cat"].value_counts() / len(strat_test_set)
3    0.350533
2    0.318798
4    0.176357
5    0.114341
1    0.039971
Name: income_cat, dtype: float64
```

Com código semelhante, você pode medir as proporções das categorias de renda no conjunto de dados completo. A Figura 2-10 compara as proporções das categorias de renda no conjunto de dados geral, no conjunto de teste gerado com amostragem estratificada e em um conjunto de teste gerado usando amostragem puramente aleatória. Como você pode ver, o conjunto de teste gerado usando amostragem estratificada tem proporções de categoria de renda quase idênticas às do conjunto de dados completo, enquanto o conjunto de teste gerado usando amostragem puramente aleatória é enviesado.

### Figura 2-10. Comparação do viés de amostragem entre amostragem estratificada e puramente aleatória

Você não usará a coluna income_cat novamente, então pode removê-la, revertendo os dados de volta ao estado original:

```python
for set_ in (strat_train_set, strat_test_set):
    set_.drop("income_cat", axis=1, inplace=True)
```

Passamos bastante tempo na geração do conjunto de teste por uma boa razão: esta é uma parte frequentemente negligenciada, mas crítica, de um projeto de aprendizado de máquina. Além disso, muitas dessas ideias serão úteis mais tarde quando discutirmos validação cruzada. Agora é hora de passar para a próxima etapa: explorar os dados.

## Explorar e Visualizar os Dados para Obter Insights

Até agora, você só deu uma olhada rápida nos dados para ter uma compreensão geral do tipo de dados que está manipulando. Agora o objetivo é ir um pouco mais fundo.

Primeiro, certifique-se de que colocou o conjunto de teste de lado e está apenas explorando o conjunto de treinamento. Além disso, se o conjunto de treinamento for muito grande, você pode querer amostrar um conjunto de exploração, para tornar as manipulações fáceis e rápidas durante a fase de exploração. Neste caso, o conjunto de treinamento é bastante pequeno, então você pode trabalhar diretamente no conjunto completo. Como você vai experimentar várias transformações no conjunto de treinamento completo, deve fazer uma cópia do original para poder reverter a ele depois:

```python
housing = strat_train_set.copy()
```

### Visualizando Dados Geográficos

Como o conjunto de dados inclui informações geográficas (latitude e longitude), é uma boa ideia criar um gráfico de dispersão de todos os distritos para visualizar os dados (Figura 2-11):

```python
housing.plot(kind="scatter", x="longitude", y="latitude", grid=True)
plt.show()
```

### Figura 2-11. Um gráfico de dispersão geográfico dos dados

Isso parece com a Califórnia, mas além disso é difícil ver qualquer padrão específico. Definir a opção alpha para 0,2 torna muito mais fácil visualizar os lugares onde há uma alta densidade de pontos de dados (Figura 2-12):

```python
housing.plot(kind="scatter", x="longitude", y="latitude", grid=True,
            alpha=0.2)
plt.show()
```

Agora está muito melhor: você pode ver claramente as áreas de alta densidade, nomeadamente a Bay Area e em torno de Los Angeles e San Diego, além de uma longa linha de áreas de densidade bastante alta no Vale Central (em particular, em torno de Sacramento e Fresno).

Nossos cérebros são muito bons em identificar padrões em imagens, mas você pode precisar brincar com os parâmetros de visualização para fazer os padrões se destacarem.

### Figura 2-12. Uma visualização melhor que destaca áreas de alta densidade

Em seguida, você olha para os preços dos imóveis (Figura 2-13). O raio de cada círculo representa a população do distrito (opção s), e a cor representa o preço (opção c). Aqui você usa um mapa de cores predefinido (opção cmap) chamado jet, que varia de azul (valores baixos) a vermelho (preços altos):

```python
housing.plot(kind="scatter", x="longitude", y="latitude", grid=True,
            s=housing["population"] / 100, label="population",
            c="median_house_value", cmap="jet", colorbar=True,
            legend=True, sharex=False, figsize=(10, 7))
plt.show()
```

Esta imagem mostra que os preços dos imóveis estão muito relacionados à localização (por exemplo, perto do oceano) e à densidade populacional, como você provavelmente já sabia. Um algoritmo de clustering deve ser útil para detectar os principais clusters e adicionar novos recursos que meçam a proximidade com os centros dos clusters. O atributo de proximidade do oceano também pode ser útil, mas no norte da Califórnia os preços dos imóveis em distritos costeiros não são muito altos, então não é uma regra simples.

### Figura 2-13. Preços de imóveis na Califórnia: vermelho é caro, azul é barato, círculos maiores indicam áreas com maior população

### Procurando por Correlações

Como o conjunto de dados não é muito grande, você pode facilmente calcular o coeficiente de correlação padrão (também chamado de r de Pearson) entre cada par de atributos usando o método corr():

```python
corr_matrix = housing.corr()
```

Agora você pode ver o quanto cada atributo está correlacionado com o valor mediano da casa:

```python
>>> corr_matrix["median_house_value"].sort_values(ascending=False)
median_house_value    1.000000
median_income         0.688380
total_rooms           0.137455
housing_median_age    0.102175
households            0.071426
total_bedrooms        0.054635
population           -0.020153
longitude            -0.050859
latitude             -0.139584
Name: median_house_value, dtype: float64
```

O coeficiente de correlação varia de –1 a 1. Quando está próximo de 1, significa que há uma forte correlação positiva; por exemplo, o valor mediano da casa tende a subir quando a renda mediana sobe. Quando o coeficiente está próximo de –1, significa que há uma forte correlação negativa; você pode ver uma pequena correlação negativa entre a latitude e o valor mediano da casa (ou seja, os preços têm uma ligeira tendência a diminuir quando você vai para o norte). Finalmente, coeficientes próximos de 0 significam que não há correlação linear.

Outra maneira de verificar a correlação entre atributos é usar a função scatter_matrix() do Pandas, que plota cada atributo numérico contra todos os outros atributos numéricos. Como agora há 11 atributos numéricos, você obteria 11² = 121 gráficos, o que não caberia em uma página—então você decide se concentrar em alguns atributos promissores que parecem mais correlacionados com o valor mediano da casa (Figura 2-14):

```python
from pandas.plotting import scatter_matrix

attributes = ["median_house_value", "median_income", "total_rooms",
              "housing_median_age"]
scatter_matrix(housing[attributes], figsize=(12, 8))
plt.show()
```

### Figura 2-14. Esta matriz de dispersão plota cada atributo numérico contra todos os outros atributos numéricos, além de um histograma dos valores de cada atributo numérico na diagonal principal (de cima à esquerda para baixo à direita)

A diagonal principal seria cheia de linhas retas se o Pandas plotasse cada variável contra si mesma, o que não seria muito útil. Então, em vez disso, o Pandas exibe um histograma de cada atributo (outras opções estão disponíveis; consulte a documentação do Pandas para mais detalhes).

Olhando para os gráficos de correlação de dispersão, parece que o atributo mais promissor para prever o valor mediano da casa é a renda mediana, então você amplia o gráfico de dispersão deles (Figura 2-15):

```python
housing.plot(kind="scatter", x="median_income", y="median_house_value",
            alpha=0.1, grid=True)
plt.show()
```

### Figura 2-15. Renda mediana versus valor mediano da casa

Este gráfico revela algumas coisas. Primeiro, a correlação é de fato bastante forte; você pode ver claramente a tendência de alta, e os pontos não estão muito dispersos. Segundo, o limite de preço que você notou anteriormente é claramente visível como uma linha horizontal em $500.000. Mas o gráfico também revela outras linhas retas menos óbvias: uma linha horizontal em torno de $450.000, outra em torno de $350.000, talvez uma em torno de $280.000, e algumas mais abaixo. Você pode querer tentar remover os distritos correspondentes para evitar que seus algoritmos aprendam a reproduzir essas peculiaridades dos dados.

**AVISO**

O coeficiente de correlação só mede correlações lineares ("à medida que x aumenta, y geralmente aumenta/diminui"). Ele pode perder completamente relações não lineares (por exemplo, "à medida que x se aproxima de 0, y geralmente aumenta"). A Figura 2-16 mostra uma variedade de conjuntos de dados junto com seu coeficiente de correlação. Observe como todos os gráficos da linha inferior têm um coeficiente de correlação igual a 0, apesar de seus eixos claramente não serem independentes: esses são exemplos de relações não lineares. Além disso, a segunda linha mostra exemplos onde o coeficiente de correlação é igual a 1 ou –1; observe que isso não tem nada a ver com a inclinação. Por exemplo, sua altura em polegadas tem um coeficiente de correlação de 1 com sua altura em pés ou em nanômetros.

### Figura 2-16. Coeficiente de correlação padrão de vários conjuntos de dados (fonte: Wikipedia; imagem de domínio público)

### Experimentando com Combinações de Atributos

Espero que as seções anteriores tenham dado uma ideia de algumas maneiras de explorar os dados e obter insights. Você identificou algumas peculiaridades nos dados que podem querer limpar antes de alimentar os dados em um algoritmo de aprendizado de máquina, e encontrou correlações interessantes entre os atributos, em particular com o atributo alvo. Você também notou que alguns atributos têm uma distribuição assimétrica à direita, então pode querer transformá-los (por exemplo, computando seu logaritmo ou raiz quadrada). Claro, sua experiência variará consideravelmente com cada projeto, mas as ideias gerais são semelhantes.

Uma última coisa que você pode querer fazer antes de preparar os dados para algoritmos de aprendizado de máquina é experimentar várias combinações de atributos. Por exemplo, o número total de quartos em um distrito não é muito útil se você não souber quantas famílias existem. O que você realmente quer é o número de quartos por família. Da mesma forma, o número total de quartos por si só não é muito útil: você provavelmente quer compará-lo com o número de quartos. E a população por família também parece ser uma combinação de atributos interessante para analisar. Você cria esses novos atributos da seguinte forma:

```python
housing["rooms_per_house"] = housing["total_rooms"] / housing["households"]
housing["bedrooms_ratio"] = housing["total_bedrooms"] / housing["total_rooms"]
housing["people_per_house"] = housing["population"] / housing["households"]
```

E então você olha para a matriz de correlação novamente:

```python
>>> corr_matrix = housing.corr()
>>> corr_matrix["median_house_value"].sort_values(ascending=False)
median_house_value    1.000000
median_income         0.688380
rooms_per_house       0.143663
total_rooms           0.137455
housing_median_age    0.102175
households            0.071426
total_bedrooms        0.054635
people_per_house     -0.038224
population           -0.020153
longitude            -0.050859
latitude             -0.139584
bedrooms_ratio       -0.256397
Name: median_house_value, dtype: float64
```

Ei, nada mal! O novo atributo bedrooms_ratio está muito mais correlacionado com o valor mediano da casa do que o número total de quartos ou quartos. Aparentemente, casas com uma proporção menor de quartos por cômodo tendem a ser mais caras. O número de quartos por família também é mais informativo que o número total de quartos em um distrito—obviamente, quanto maiores as casas, mais caras elas são.

Esta rodada de exploração não precisa ser absolutamente completa; o ponto é começar com o pé direito e obter rapidamente insights que o ajudarão a obter um primeiro protótipo razoavelmente bom. Mas este é um processo iterativo: uma vez que você tenha um protótipo funcionando, pode analisar sua saída para obter mais insights e voltar a esta etapa de exploração.

## Preparar os Dados para Algoritmos de Aprendizado de Máquina

É hora de preparar os dados para seus algoritmos de aprendizado de máquina. Em vez de fazer isso manualmente, você deve escrever funções para esse propósito, por várias boas razões:

- Isso permitirá que você reproduza essas transformações facilmente em qualquer conjunto de dados (por exemplo, na próxima vez que obter um conjunto de dados novo).
- Você construirá gradualmente uma biblioteca de funções de transformação que pode reutilizar em projetos futuros.
- Você pode usar essas funções em seu sistema ao vivo para transformar novos dados antes de alimentá-los em seus algoritmos.
- Isso tornará possível experimentar várias transformações e ver qual combinação funciona melhor.

Mas primeiro, volte a um conjunto de treinamento limpo (copiando strat_train_set mais uma vez). Você também deve separar os preditores e os rótulos, já que não necessariamente quer aplicar as mesmas transformações aos preditores e aos valores alvo (note que drop() cria uma cópia dos dados e não afeta strat_train_set):

```python
housing = strat_train_set.drop("median_house_value", axis=1)
housing_labels = strat_train_set["median_house_value"].copy()
```

### Limpar os Dados

A maioria dos algoritmos de aprendizado de máquina não consegue trabalhar com recursos ausentes, então você precisará cuidar disso. Por exemplo, você notou anteriormente que o atributo total_bedrooms tem alguns valores ausentes. Você tem três opções para corrigir isso:

1. **Eliminar os distritos correspondentes.**
2. **Eliminar todo o atributo.**
3. **Definir os valores ausentes para algum valor (zero, a média, a mediana, etc.). Isso é chamado de imputação.**

Você pode realizar isso facilmente usando os métodos dropna(), drop() e fillna() do DataFrame do Pandas:

```python
housing.dropna(subset=["total_bedrooms"], inplace=True)  # opção 1
housing.drop("total_bedrooms", axis=1)  # opção 2
median = housing["total_bedrooms"].median()  # opção 3
housing["total_bedrooms"].fillna(median, inplace=True)
```

Você decide ir com a opção 3, pois é a menos destrutiva, mas em vez do código anterior, você usará uma classe útil do Scikit-Learn: SimpleImputer. O benefício é que ele armazenará o valor mediano de cada recurso: isso tornará possível imputar valores ausentes não apenas no conjunto de treinamento, mas também no conjunto de validação, no conjunto de teste e em quaisquer novos dados alimentados ao modelo. Para usá-lo, primeiro você precisa criar uma instância do SimpleImputer, especificando que deseja substituir os valores ausentes de cada atributo pela mediana desse atributo:

```python
from sklearn.impute import SimpleImputer

imputer = SimpleImputer(strategy="median")
```

Como a mediana só pode ser computada em atributos numéricos, você precisa criar uma cópia dos dados com apenas os atributos numéricos (isso excluirá o atributo de texto ocean_proximity):

```python
housing_num = housing.select_dtypes(include=[np.number])
```

Agora você pode ajustar a instância do imputer aos dados de treinamento usando o método fit():

```python
imputer.fit(housing_num)
```

O imputer simplesmente computou a mediana de cada atributo e armazenou o resultado em sua variável de instância statistics_. Apenas o atributo total_bedrooms tinha valores ausentes, mas você não pode ter certeza de que não haverá valores ausentes em novos dados após o sistema entrar em produção, então é mais seguro aplicar o imputer a todos os atributos numéricos:

```python
>>> imputer.statistics_
array([-118.51 , 34.26 , 29. , 2125. , 434. , 1167. , 408. , 3.5385])
>>> housing_num.median().values
array([-118.51 , 34.26 , 29. , 2125. , 434. , 1167. , 408. , 3.5385])
```

Agora você pode usar este imputer "treinado" para transformar o conjunto de treinamento substituindo os valores ausentes pelas medianas aprendidas:

```python
X = imputer.transform(housing_num)
```

Valores ausentes também podem ser substituídos pelo valor médio (strategy="mean"), ou pelo valor mais frequente (strategy="most_frequent"), ou por um valor constante (strategy="constant", fill_value=...). As duas últimas estratégias suportam dados não numéricos.

**DICA**

Há também imputadores mais poderosos disponíveis no pacote sklearn.impute (apenas para recursos numéricos):

- KNNImputer substitui cada valor ausente pela média dos valores dos k-vizinhos mais próximos para esse recurso. A distância é baseada em todos os recursos disponíveis.
- IterativeImputer treina um modelo de regressão por recurso para prever os valores ausentes com base em todos os outros recursos disponíveis. Ele então treina o modelo novamente nos dados atualizados e repete o processo várias vezes, melhorando os modelos e os valores de substituição a cada iteração.

### Projetando o Scikit-Learn

A API do Scikit-Learn é notavelmente bem projetada. Estes são os principais princípios de design:

- **Consistência**: Todos os objetos compartilham uma interface consistente e simples:
  - **Estimadores**: Qualquer objeto que pode estimar alguns parâmetros com base em um conjunto de dados é chamado de estimador (por exemplo, um SimpleImputer é um estimador). A estimativa em si é realizada pelo método fit(), e ele recebe um conjunto de dados como parâmetro, ou dois para algoritmos de aprendizado supervisionado—o segundo conjunto de dados contém os rótulos. Qualquer outro parâmetro necessário para guiar o processo de estimativa é considerado um hiperparâmetro (como a estratégia de um SimpleImputer), e deve ser definido como uma variável de instância (geralmente via um parâmetro do construtor).
  - **Transformadores**: Alguns estimadores (como um SimpleImputer) também podem transformar um conjunto de dados; estes são chamados de transformadores. Mais uma vez, a API é simples: a transformação é realizada pelo método transform() com o conjunto de dados a ser transformado como parâmetro. Ele retorna o conjunto de dados transformado. Essa transformação geralmente depende dos parâmetros aprendidos, como é o caso de um SimpleImputer. Todos os transformadores também têm um método conveniente chamado fit_transform(), que é equivalente a chamar fit() e depois transform() (mas às vezes fit_transform() é otimizado e roda muito mais rápido).
  - **Preditores**: Finalmente, alguns estimadores, dado um conjunto de dados, são capazes de fazer previsões; eles são chamados de preditores. Por exemplo, o modelo LinearRegression no capítulo anterior era um preditor: dado o PIB per capita de um país, ele previu a satisfação de vida. Um preditor tem um método predict() que recebe um conjunto de dados de novas instâncias e retorna um conjunto de dados de previsões correspondentes. Ele também tem um método score() que mede a qualidade das previsões, dado um conjunto de teste (e os rótulos correspondentes, no caso de algoritmos de aprendizado supervisionado).

- **Inspeção**: Todos os hiperparâmetros do estimador são acessíveis diretamente via variáveis de instância públicas (por exemplo, imputer.strategy), e todos os parâmetros aprendidos do estimador são acessíveis via variáveis de instância públicas com um sublinhado sufixo (por exemplo, imputer.statistics_).

- **Não proliferação de classes**: Conjuntos de dados são representados como arrays NumPy ou matrizes esparsas SciPy, em vez de classes caseiras. Hiperparâmetros são apenas strings ou números Python regulares.

- **Composição**: Blocos de construção existentes são reutilizados tanto quanto possível. Por exemplo, é fácil criar um estimador Pipeline a partir de uma sequência arbitrária de transformadores seguidos por um estimador final, como você verá.

- **Padrões sensíveis**: O Scikit-Learn fornece valores padrão razoáveis para a maioria dos parâmetros, tornando fácil criar rapidamente um sistema de trabalho básico.

Os transformadores do Scikit-Learn produzem arrays NumPy (ou às vezes matrizes esparsas SciPy) mesmo quando são alimentados com DataFrames do Pandas como entrada. Então, a saída de imputer.transform(housing_num) é um array NumPy: X não tem nomes de colunas nem índice. Felizmente, não é muito difícil envolver X em um DataFrame e recuperar os nomes das colunas e o índice de housing_num:

```python
housing_tr = pd.DataFrame(X, columns=housing_num.columns,
                         index=housing_num.index)
```

### Lidando com Atributos de Texto e Categóricos

Até agora lidamos apenas com atributos numéricos, mas seus dados também podem conter atributos de texto. Neste conjunto de dados, há apenas um: o atributo ocean_proximity. Vamos olhar para seus valores nas primeiras instâncias:

```python
>>> housing_cat = housing[["ocean_proximity"]]
>>> housing_cat.head(8)
ocean_proximity
13096       NEAR BAY
14973    <1H OCEAN
3785         INLAND
14689         INLAND
20507    NEAR OCEAN
1286         INLAND
18078    <1H OCEAN
4396       NEAR BAY
```

Não é texto arbitrário: há um número limitado de valores possíveis, cada um representando uma categoria. Portanto, este atributo é um atributo categórico. A maioria dos algoritmos de aprendizado de máquina prefere trabalhar com números, então vamos converter essas categorias de texto para números. Para isso, podemos usar a classe OrdinalEncoder do Scikit-Learn:

```python
from sklearn.preprocessing import OrdinalEncoder

ordinal_encoder = OrdinalEncoder()
housing_cat_encoded = ordinal_encoder.fit_transform(housing_cat)
```

Aqui estão os primeiros valores codificados em housing_cat_encoded:

```python
>>> housing_cat_encoded[:8]
array([[3.],
       [0.],
       [1.],
       [1.],
       [4.],
       [1.],
       [0.],
       [3.]])
```

Você pode obter a lista de categorias usando a variável de instância categories_. É uma lista contendo um array 1D de categorias para cada atributo categórico (neste caso, uma lista contendo um único array, pois há apenas um atributo categórico):

```python
>>> ordinal_encoder.categories_
[array(['<1H OCEAN', 'INLAND', 'ISLAND', 'NEAR BAY', 'NEAR OCEAN'],
      dtype=object)]
```

Um problema com esta representação é que os algoritmos de ML assumirão que dois valores próximos são mais semelhantes que dois valores distantes. Isso pode ser bom em alguns casos (por exemplo, para categorias ordenadas como "ruim", "médio", "bom" e "excelente"), mas obviamente não é o caso da coluna ocean_proximity (por exemplo, as categorias 0 e 4 são claramente mais semelhantes que as categorias 0 e 1). Para corrigir isso, uma solução comum é criar um atributo binário por categoria: um atributo igual a 1 quando a categoria é "<1H OCEAN" (e 0 caso contrário), outro atributo igual a 1 quando a categoria é "INLAND" (e 0 caso contrário), e assim por diante. Isso é chamado de codificação one-hot, porque apenas um atributo será igual a 1 (quente), enquanto os outros serão 0 (frios). Os novos atributos às vezes são chamados de atributos dummy. O Scikit-Learn fornece uma classe OneHotEncoder para converter valores categóricos em vetores one-hot:

```python
from sklearn.preprocessing import OneHotEncoder

cat_encoder = OneHotEncoder()
housing_cat_1hot = cat_encoder.fit_transform(housing_cat)
```

Por padrão, a saída de um OneHotEncoder é uma matriz esparsa SciPy, em vez de um array NumPy:

```python
>>> housing_cat_1hot
<16512x5 sparse matrix of type '<class 'numpy.float64'>'
    with 16512 stored elements in Compressed Sparse Row format>
```

Uma matriz esparsa é uma representação muito eficiente para matrizes que contêm principalmente zeros. Na verdade, internamente ela armazena apenas os valores não nulos e suas posições. Quando um atributo categórico tem centenas ou milhares de categorias, codificá-lo em one-hot resulta em uma matriz muito grande cheia de zeros, exceto por um único 1 por linha. Nesse caso, uma matriz esparsa é exatamente o que você precisa: ela economizará muita memória e acelerará os cálculos. Você pode usar uma matriz esparsa principalmente como um array 2D normal, mas se quiser convertê-la em um array NumPy (denso), basta chamar o método toarray():

```python
>>> housing_cat_1hot.toarray()
array([[0., 0., 0., 1., 0.],
       [1., 0., 0., 0., 0.],
       [0., 1., 0., 0., 0.],
       ...,
       [0., 0., 0., 0., 1.],
       [1., 0., 0., 0., 0.],
       [0., 0., 0., 0., 1.]])
```

Alternativamente, você pode definir sparse=False ao criar o OneHotEncoder, caso em que o método transform() retornará um array NumPy (denso) diretamente.

Como com o OrdinalEncoder, você pode obter a lista de categorias usando a variável de instância categories_ do codificador:

```python
>>> cat_encoder.categories_
[array(['<1H OCEAN', 'INLAND', 'ISLAND', 'NEAR BAY', 'NEAR OCEAN'],
      dtype=object)]
```

O Pandas tem uma função chamada get_dummies(), que também converte cada recurso categórico em uma representação one-hot, com um recurso binário por categoria:

```python
>>> df_test = pd.DataFrame({"ocean_proximity": ["INLAND", "NEAR BAY"]})
>>> pd.get_dummies(df_test)
ocean_proximity_INLAND  ocean_proximity_NEAR BAY
0                      1                         0
1                      0                         1
```

Parece legal e simples, então por que não usá-lo em vez de OneHotEncoder? Bem, a vantagem do OneHotEncoder é que ele lembra em quais categorias foi treinado. Isso é muito importante porque, uma vez que seu modelo esteja em produção, ele deve ser alimentado exatamente com os mesmos recursos que durante o treinamento: nem mais, nem menos. Veja o que nosso cat_encoder treinado produz quando o fazemos transformar o mesmo df_test (usando transform(), não fit_transform()):

```python
>>> cat_encoder.transform(df_test)
array([[0., 1., 0., 0., 0.],
       [0., 0., 0., 1., 0.]])
```

Viu a diferença? get_dummies() viu apenas duas categorias, então produziu duas colunas, enquanto OneHotEncoder produziu uma coluna por categoria aprendida, na ordem correta. Além disso, se você alimentar get_dummies() com um DataFrame contendo uma categoria desconhecida (por exemplo, "<2H OCEAN"), ele gerará alegremente uma coluna para ela:

```python
>>> df_test_unknown = pd.DataFrame({"ocean_proximity": ["<2H OCEAN",
...                                                   "ISLAND"]})
>>> pd.get_dummies(df_test_unknown)
ocean_proximity_<2H OCEAN  ocean_proximity_ISLAND
0                         1                      0
1                         0                      1
```

Mas OneHotEncoder é mais esperto: ele detectará a categoria desconhecida e levantará uma exceção. Se preferir, você pode definir o hiperparâmetro handle_unknown como "ignore", caso em que ele representará a categoria desconhecida com zeros:

```python
>>> cat_encoder.handle_unknown = "ignore"
>>> cat_encoder.transform(df_test_unknown)
array([[0., 0., 0., 0., 0.],
       [0., 0., 1., 0., 0.]])
```

**DICA**

Se um atributo categórico tiver um grande número de valores possíveis (por exemplo, código de país, profissão, espécie), então a codificação one-hot resultará em um grande número de recursos de entrada. Isso pode desacelerar o treinamento e degradar o desempenho. Se isso acontecer, você pode querer substituir a entrada categórica por recursos numéricos úteis relacionados às categorias: por exemplo, você pode substituir o recurso ocean_proximity pela distância até o oceano (da mesma forma, um código de país poderia ser substituído pela população do país e PIB per capita). Alternativamente, você pode usar um dos codificadores fornecidos pelo pacote category_encoders no GitHub. Ou, ao lidar com redes neurais, você pode substituir cada categoria por um vetor de baixa dimensão aprendível chamado embedding. Este é um exemplo de aprendizado de representação (veja os Capítulos 13 e 17 para mais detalhes).

Quando você ajusta qualquer estimador do Scikit-Learn usando um DataFrame, o estimador armazena os nomes das colunas no atributo feature_names_in_. O Scikit-Learn então garante que qualquer DataFrame passado para este estimador depois disso (por exemplo, para transform() ou predict()) tenha os mesmos nomes de colunas. Os transformadores também fornecem um método get_feature_names_out() que você pode usar para construir um DataFrame em torno da saída do transformador:

```python
>>> cat_encoder.feature_names_in_
array(['ocean_proximity'], dtype=object)
>>> cat_encoder.get_feature_names_out()
array(['ocean_proximity_<1H OCEAN', 'ocean_proximity_INLAND',
       'ocean_proximity_ISLAND', 'ocean_proximity_NEAR BAY',
       'ocean_proximity_NEAR OCEAN'], dtype=object)
>>> df_output = pd.DataFrame(cat_encoder.transform(df_test_unknown),
...                        columns=cat_encoder.get_feature_names_out(),
...                        index=df_test_unknown.index)
```

### Dimensionamento e Transformação de Recursos

Uma das transformações mais importantes que você precisa aplicar aos seus dados é o dimensionamento de recursos. Com poucas exceções, os algoritmos de aprendizado de máquina não têm um bom desempenho quando os atributos numéricos de entrada têm escalas muito diferentes. Este é o caso dos dados de habitação: o número total de quartos varia de cerca de 6 a 39.320, enquanto as rendas medianas variam apenas de 0 a 15. Sem nenhum dimensionamento, a maioria dos modelos será tendenciosa para ignorar a renda mediana e se concentrar mais no número de quartos.

**AVISO**

Como com todos os estimadores, é importante ajustar os escalonadores apenas aos dados de treinamento: nunca use fit() ou fit_transform() para nada além do conjunto de treinamento. Uma vez que você tenha um escalonador treinado, você pode então usá-lo para transformar() qualquer outro conjunto, incluindo o conjunto de validação, o conjunto de teste e novos dados. Observe que, embora os valores do conjunto de treinamento sempre sejam dimensionados para o intervalo especificado, se novos dados contiverem outliers, eles podem acabar dimensionados fora do intervalo. Se você quiser evitar isso, basta definir o hiperparâmetro clip como True.

O dimensionamento min-max (muitas pessoas chamam isso de normalização) é o mais simples: para cada atributo, os valores são deslocados e redimensionados para que acabem variando de 0 a 1. Isso é feito subtraindo o valor mínimo e dividindo pela diferença entre o mínimo e o máximo. O Scikit-Learn fornece um transformador chamado MinMaxScaler para isso. Ele tem um hiperparâmetro feature_range que permite alterar o intervalo se, por algum motivo, você não quiser 0–1 (por exemplo, redes neurais funcionam melhor com entradas de média zero, então um intervalo de –1 a 1 é preferível). É bastante fácil de usar:

```python
from sklearn.preprocessing import MinMaxScaler

min_max_scaler = MinMaxScaler(feature_range=(-1, 1))
housing_num_min_max_scaled = min_max_scaler.fit_transform(housing_num)
```

A padronização é diferente: primeiro subtrai o valor médio (para que os valores padronizados tenham média zero), depois divide o resultado pelo desvio padrão (para que os valores padronizados tenham desvio padrão igual a 1). Ao contrário do dimensionamento min-max, a padronização não restringe os valores a um intervalo específico. No entanto, a padronização é muito menos afetada por outliers. Por exemplo, suponha que um distrito tenha uma renda mediana igual a 100 (por engano), em vez do usual 0–15. O dimensionamento min-max para o intervalo 0–1 mapearia esse outlier para 1 e esmagaria todos os outros valores para 0–0,15, enquanto a padronização não seria muito afetada. O Scikit-Learn fornece um transformador chamado StandardScaler para padronização:

```python
from sklearn.preprocessing import StandardScaler

std_scaler = StandardScaler()
housing_num_std_scaled = std_scaler.fit_transform(housing_num)
```

**DICA**

Se você quiser dimensionar uma matriz esparsa sem convertê-la primeiro em uma matriz densa, você pode usar um StandardScaler com seu hiperparâmetro with_mean definido como False: ele apenas dividirá os dados pelo desvio padrão, sem subtrair a média (pois isso quebraria a esparsidade).

Quando a distribuição de um recurso tem uma cauda pesada (ou seja, quando valores distantes da média não são exponencialmente raros), tanto o dimensionamento min-max quanto a padronização esmagarão a maioria dos valores em um intervalo pequeno. Os modelos de aprendizado de máquina geralmente não gostam nada disso, como você verá no Capítulo 4. Então, antes de dimensionar o recurso, você deve primeiro transformá-lo para reduzir a cauda pesada e, se possível, tornar a distribuição aproximadamente simétrica. Por exemplo, uma maneira comum de fazer isso para recursos positivos com uma cauda pesada à direita é substituir o recurso por sua raiz quadrada (ou elevar o recurso a uma potência entre 0 e 1). Se o recurso tiver uma cauda realmente longa e pesada, como uma distribuição de lei de potência, então substituir o recurso por seu logaritmo pode ajudar. Por exemplo, o recurso population segue aproximadamente uma lei de potência: distritos com 10.000 habitantes são apenas 10 vezes menos frequentes que distritos com 1.000 habitantes, não exponencialmente menos frequentes. A Figura 2-17 mostra como este recurso fica muito melhor quando você calcula seu log: ele está muito próximo de uma distribuição Gaussiana (ou seja, em forma de sino