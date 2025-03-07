---
Atualizado: 2025-03-07  15.47
Criado: 2025-03-07  14.55
---
Claro! Vou traduzir o conteúdo do arquivo para o português. Aqui está a tradução:

---

# Capítulo 2. Projeto de Machine Learning de Ponta a Ponta

Neste capítulo, você trabalhará em um projeto de exemplo de ponta a ponta, fingindo ser um cientista de dados recém-contratado em uma empresa de imóveis. Este exemplo é fictício; o objetivo é ilustrar as principais etapas de um projeto de machine learning, e não aprender algo sobre o mercado imobiliário. Aqui estão as principais etapas que percorreremos:

1. Olhar para o panorama geral.
2. Obter os dados.
3. Explorar e visualizar os dados para obter insights.
4. Preparar os dados para algoritmos de machine learning.
5. Selecionar um modelo e treiná-lo.
6. Ajustar o modelo.
7. Apresentar sua solução.
8. Lançar, monitorar e manter seu sistema.

## Trabalhando com Dados Reais

Quando você está aprendendo sobre machine learning, é melhor experimentar com dados do mundo real, e não com conjuntos de dados artificiais. Felizmente, existem milhares de conjuntos de dados abertos para escolher, abrangendo todos os tipos de domínios. Aqui estão alguns lugares onde você pode procurar dados:

- Repositórios populares de dados abertos:
  - OpenML.org
  - Kaggle.com
  - PapersWithCode.com
  - Repositório de Machine Learning da UC Irvine
  - Conjuntos de dados da AWS da Amazon
  - Conjuntos de dados do TensorFlow
  - Portais meta (eles listam repositórios de dados abertos):
    - DataPortals.org
    - OpenDataMonitor.eu
  - Outras páginas que listam muitos repositórios de dados abertos populares:
    - Lista de conjuntos de dados de machine learning da Wikipedia
    - Quora.com
    - O subreddit de conjuntos de dados

Neste capítulo, usaremos o conjunto de dados de preços de habitação na Califórnia do repositório StatLib[1] (veja a Figura 2-1). Este conjunto de dados é baseado em dados do censo da Califórnia de 1990. Não é exatamente recente (uma boa casa na Bay Area ainda era acessível na época), mas tem muitas qualidades para aprendizado, então vamos fingir que são dados recentes. Para fins de ensino, adicionei um atributo categórico e removi alguns recursos.

---

# Olhar para o Panorama Geral

Bem-vindo à Corporação de Machine Learning de Habitação! Sua primeira tarefa é usar os dados do censo da Califórnia para construir um modelo de preços de habitação no estado. Esses dados incluem métricas como população, renda média e preço médio de habitação para cada grupo de blocos na Califórnia. Grupos de blocos são a menor unidade geográfica para a qual o US Census Bureau publica dados de amostra (um grupo de blocos normalmente tem uma população de 600 a 3.000 pessoas). Vou chamá-los de "distritos" para simplificar.

Seu modelo deve aprender com esses dados e ser capaz de prever o preço médio de habitação em qualquer distrito, dadas todas as outras métricas.

---

# DICA

Como você é um cientista de dados bem organizado, a primeira coisa que deve fazer é pegar sua lista de verificação de projetos de machine learning. Você pode começar com a do Apêndice A; ela deve funcionar razoavelmente bem para a maioria dos projetos de machine learning, mas certifique-se de adaptá-la às suas necessidades. Neste capítulo, passaremos por muitos itens da lista de verificação, mas também pularmos alguns, seja porque são autoexplicativos ou porque serão discutidos em capítulos posteriores.

## Definir o Problema

A primeira pergunta a fazer ao seu chefe é qual exatamente é o objetivo do negócio. Construir um modelo provavelmente não é o objetivo final. Como a empresa espera usar e se beneficiar desse modelo? Saber o objetivo é importante porque ele determinará como você enquadrará o problema, quais algoritmos selecionará, qual medida de desempenho usará para avaliar seu modelo e quanto esforço gastará ajustando-o.

Seu chefe responde que a saída do seu modelo (uma previsão do preço médio de habitação de um distrito) será alimentada em outro sistema de machine learning (veja a Figura 2-2), junto com muitos outros sinais.\(^2\) Esse sistema downstream determinará se vale a pena investir em uma determinada área. Acertar isso é crítico, pois afeta diretamente a receita.

A próxima pergunta a fazer ao seu chefe é como é a solução atual (se houver). A situação atual geralmente lhe dará uma referência de desempenho, bem como insights sobre como resolver o problema. Seu chefe responde que os preços de habitação dos distritos são atualmente estimados manualmente por especialistas: uma equipe coleta informações atualizadas sobre um distrito, e quando não conseguem obter o preço médio de habitação, eles o estimam usando regras complexas.

---

_Seu componente    Outros sinais_

_Componentes upstream_

Precificação de distrito

Análise de investimento

Dados do distrito

Preços do distrito

_Figura 2-2. Um pipeline de machine learning para investimentos imobiliários_

Isso é caro e demorado, e suas estimativas não são ótimas; nos casos em que conseguem descobrir o preço médio de habitação real, muitas vezes percebem que suas estimativas estavam erradas em mais de 30%. É por isso que a empresa acha que seria útil treinar um modelo para prever o preço médio de habitação de um distrito, dados outros dados sobre esse distrito. Os dados do censo parecem um ótimo conjunto de dados para explorar para esse propósito, já que incluem os preços médios de habitação de milhares de distritos, além de outros dados.

---

# PIPELINES

Uma sequência de componentes de processamento de dados é chamada de pipeline de dados.  
Pipelines são muito comuns em sistemas de machine learning, pois há muitos dados para manipular e muitas transformações de dados para aplicar.

Os componentes geralmente são executados de forma assíncrona. Cada componente puxa uma grande quantidade de dados, processa-os e gera o resultado em outro armazenamento de dados. Então, algum tempo depois, o próximo componente no pipeline puxa esses dados e gera sua própria saída. Cada componente é bastante autônomo: a interface entre os componentes é simplesmente o armazenamento de dados. Isso torna o sistema simples de entender (com a ajuda de um gráfico de fluxo de dados), e diferentes equipes podem se concentrar em diferentes componentes. Além disso, se um componente falhar, os componentes downstream geralmente podem continuar a funcionar normalmente (pelo menos por um tempo) usando apenas a última saída do componente quebrado. Isso torna a arquitetura bastante robusta.

Por outro lado, um componente quebrado pode passar despercebido por algum tempo se o monitoramento adequado não for implementado. Os dados ficam desatualizados e o desempenho geral do sistema cai.

Com todas essas informações, você agora está pronto para começar a projetar seu sistema. Primeiro, determine que tipo de supervisão de treinamento o modelo precisará: é uma tarefa de aprendizado supervisionado, não supervisionado, semi-supervisionado, auto-supervisionado ou de reforço? E é uma tarefa de classificação, regressão ou algo mais? Você deve usar técnicas de aprendizado em lote ou online? Antes de continuar, pause e tente responder a essas perguntas por si mesmo.

Você encontrou as respostas? Vamos ver. Esta é claramente uma tarefa típica de aprendizado supervisionado, pois o modelo pode ser treinado com exemplos *rotulados* (cada instância vem com a saída esperada, ou seja, o preço médio de habitação do distrito). É uma tarefa típica de regressão, pois o modelo será solicitado a prever um valor. Mais especificamente, este é um problema de *regressão múltipla*, pois o sistema usará vários recursos para fazer uma previsão (a população do distrito, a renda média, etc.). Também é um problema de regressão *univariada*, pois estamos tentando prever apenas um único valor para cada distrito. Se estivéssemos tentando prever vários valores por distrito, seria um problema de regressão multivariada. Finalmente, não há um fluxo contínuo de dados entrando no sistema, não há necessidade particular de se ajustar rapidamente a mudanças nos dados, e os dados são pequenos o suficiente para caber na memória, então o aprendizado em lote simples deve funcionar bem.

**DICA**

Se os dados fossem enormes, você poderia dividir seu trabalho de aprendizado em lote em vários servidores (usando a técnica MapReduce) ou usar uma técnica de aprendizado online.

### Selecionar uma Medida de Desempenho

Seu próximo passo é selecionar uma medida de desempenho. Uma medida de desempenho típica para problemas de regressão é o erro quadrático médio (RMSE). Ele dá uma ideia de quanto erro o sistema normalmente comete em suas previsões, com um peso maior dado a erros grandes. A Equação 2-1 mostra a fórmula matemática para calcular o RMSE.

_Equação 2-1. Erro quadrático médio (RMSE)_

\[RMSE(X, h) = \sqrt{\frac{1}{m} \sum_{i=1}^{m} \left( h(x^{(i)}) - y^{(i)} \right)^2}\]

---

# NOTAÇÕES

Esta equação introduz várias notações muito comuns em machine learning que usarei ao longo deste livro:

- \( m \) é o número de instâncias no conjunto de dados em que você está medindo o RMSE.
  - Por exemplo, se você está avaliando o RMSE em um conjunto de validação de 2.000 distritos, então \( m = 2.000 \).

- \( x^{(i)} \) é um vetor de todos os valores das características (excluindo o rótulo) da \( i^{th} \) instância no conjunto de dados, e \( y^{(i)} \) é seu rótulo (o valor de saída desejado para essa instância).
  - Por exemplo, se o primeiro distrito no conjunto de dados estiver localizado na longitude \(-118.29^\circ\), latitude \( 33.91^\circ \), e tiver 1.416 habitantes com uma renda média de \( \$38.372 \), e o valor médio da casa for \( \$156.400 \) (ignorando outras características por enquanto), então:
\[x^{(1)} =
\begin{pmatrix}
-118.29 \\
33.91 \\
1.416 \\
38.372
\end{pmatrix}\]
e:
\[y^{(1)} = 156.400\]

- \( X \) é uma matriz contendo todos os valores das características (excluindo rótulos) de todas as instâncias no conjunto de dados. Há uma linha por instância, e a \( i^{th} \) linha é igual à transposta de \( x^{(i)} \), notada \( (x^{(i)})^{\top} \).\(^3\)
  - Por exemplo, se o primeiro distrito for como acabamos de descrever, então a matriz \( X \) se parece com isso:

---

\[X = \begin{pmatrix}
(x^{(1)})^T \\
(x^{(2)})^T \\
\vdots \\
(x^{(1999)})^T \\
(x^{(2000)})^T
\end{pmatrix} = 
\begin{pmatrix}
-118.29 & 33.91 & 1.416 & 38.372 \\
\vdots & \vdots & \vdots & \vdots \\
\end{pmatrix}\]

- \( h \) é a função de previsão do seu sistema, também chamada de hipótese. Quando seu sistema recebe um vetor de características \( x^{(i)} \) de uma instância, ele gera um valor previsto \( \hat{y}^{(i)} = h(x^{(i)}) \) para essa instância (\(\hat{y}\) é pronunciado "y-chapéu").

- Por exemplo, se seu sistema prever que o preço médio de habitação no primeiro distrito é \$158.400, então \( y^{(1)} = h(x^{(1)}) = 158.400 \). O erro de previsão para este distrito é \( \hat{y}^{(1)} - y^{(1)} = 2.000 \).

- RMSE(\( X,h \)) é a função de custo medida no conjunto de exemplos usando sua hipótese \( h \).

Usamos fonte itálica minúscula para valores escalares (como \( m \) ou \( y^{(i)}\)) e nomes de funções (como \( h \)), fonte em negrito minúscula para vetores (como \( x^{(i)}) \), e fonte em negrito maiúscula para matrizes (como \( X \)). 

Embora o RMSE seja geralmente a medida de desempenho preferida para tarefas de regressão, em alguns contextos você pode preferir usar outra função. Por exemplo, se houver muitos distritos discrepantes. Nesse caso, você pode considerar usar o erro absoluto médio (MAE, também chamado de desvio absoluto médio), mostrado na Equação 2-2:

Equação 2-2. Erro absoluto médio (MAE)

\[MAE(X, h) = \frac{1}{m} \sum_{i=1}^{m} |h(x^{(i)}) - y^{(i)}|\]

---

Ambos o RMSE e o MAE são maneiras de medir a distância entre dois vetores: o vetor de previsões e o vetor de valores alvo. Várias medidas de distância, ou normas, são possíveis:

- Calcular a raiz de uma soma de quadrados (RMSE) corresponde à norma euclidiana: esta é a noção de distância com a qual todos estamos familiarizados. Também é chamada de norma ℓ2, notada || · ||2 (ou apenas || · ||).

- Calcular a soma de valores absolutos (MAE) corresponde à norma ℓ1, notada || · ||1. Isso às vezes é chamado de norma de Manhattan porque mede a distância entre dois pontos em uma cidade se você só puder viajar ao longo de quarteirões ortogonais.

- Mais geralmente, a norma ℓk de um vetor v contendo n elementos é definida como ||v||k = (|v1|k + |v2|k + ... + |vn|k)^{1/k}. ℓ0 dá o número de elementos diferentes de zero no vetor, e ℓ∞ dá o valor absoluto máximo no vetor.

Quanto maior o índice da norma, mais ela se concentra em valores grandes e negligencia os pequenos. É por isso que o RMSE é mais sensível a valores discrepantes do que o MAE. Mas quando os valores discrepantes são exponencialmente raros (como em uma curva em forma de sino), o RMSE funciona muito bem e geralmente é preferido.

## Verificar as Suposições

Por fim, é uma boa prática listar e verificar as suposições que foram feitas até agora (por você ou por outros); isso pode ajudá-lo a detectar problemas sérios desde o início. Por exemplo, os preços dos distritos que seu sistema gera serão alimentados em um sistema de machine learning downstream, e você assume que esses preços serão usados como estão. Mas e se o sistema downstream converter os preços em categorias (por exemplo, "barato", "médio" ou "caro") e então usar essas categorias em vez dos próprios preços? Nesse caso, acertar o preço perfeitamente não é importante; seu sistema só precisa acertar a categoria. Se for esse o caso, então o problema deveria ter sido enquadrado como uma tarefa de classificação, não de regressão. Você não quer descobrir isso depois de trabalhar em um sistema de regressão por meses.

Felizmente, após conversar com a equipe responsável pelo sistema downstream, você está confiante de que eles realmente precisam dos preços reais, e não apenas de categorias. Ótimo! Você está pronto, as luzes estão verdes, e você pode começar a codificar agora!

## Obter os Dados

É hora de sujar as mãos. Não hesite em pegar seu laptop e seguir os exemplos de código. Como mencionei no prefácio, todos os exemplos de código neste livro são de código aberto e estão disponíveis online como notebooks Jupyter, que são documentos interativos contendo texto, imagens e trechos de código executável (Python no nosso caso). Neste livro, assumirei que você está executando esses notebooks no Google Colab, um serviço gratuito que permite executar qualquer notebook Jupyter diretamente online, sem precisar instalar nada em sua máquina. Se você quiser usar outra plataforma online (por exemplo, Kaggle) ou se quiser instalar tudo localmente em sua própria máquina, consulte as instruções na página do GitHub do livro.

### Executando os Exemplos de Código Usando o Google Colab

Primeiro, abra um navegador da web e visite https://homl.info/colab3: isso o levará ao Google Colab e exibirá a lista de notebooks Jupyter para este livro (veja a Figura 2-3). Você encontrará um notebook por capítulo, além de alguns notebooks extras e tutoriais para NumPy, Matplotlib, Pandas, álgebra linear e cálculo diferencial. Por exemplo, se você clicar em _02_end_to_end_machine_learning_project.ipynb_, o notebook do Capítulo 2 será aberto no Google Colab (veja a Figura 2-4).

Um notebook Jupyter é composto por uma lista de células. Cada célula contém código executável ou texto. Tente clicar duas vezes na primeira célula de texto (que contém a frase "Bem-vindo à Corporação de Machine Learning de Habitação!"). Isso abrirá a célula para edição. Observe que os notebooks Jupyter usam sintaxe Markdown para formatação (por exemplo, **negrito**, *itálico*, # Título, [url](texto do link), etc.). Tente modificar este texto e pressione Shift-Enter para ver o resultado.

Figura 2-3. Lista de notebooks no Google Colab

---

**Capítulo 2 – Projeto de Machine Learning de Ponta a Ponta**

*Bem-vindo à Corporação de Machine Learning de Habitação! Sua tarefa é prever os valores médios das casas em distritos da Califórnia, dados vários recursos desses distritos.*

*Este notebook contém todo o código de exemplo e as soluções para os exercícios do capítulo 2.*

- **Configuração**
  - Células de texto: clique duas vezes para editar
  - Primeiro, vamos importar alguns módulos comuns, garantir que o Matplotlib plote figuras inline e preparar uma função para salvar as figuras.
    - Células de código: clique para editar
  [ ] # Python E3.7 é necessário
    - Import sys
    - assert sys.version_info >= (3, 7)

*Figura 2-4. Seu notebook no Google Colab*

Em seguida, crie uma nova célula de código selecionando Inserir → "Célula de código" no menu. Alternativamente, você pode clicar no botão + Código na barra de ferramentas ou passar o mouse sobre a parte inferior de uma célula até ver + Código e + Texto aparecerem, então clique em + Código. Na nova célula de código, digite algum código Python, como print("Hello World"), e pressione Shift-Enter para executar este código (ou clique no botão > no lado esquerdo da célula).

Se você não estiver logado na sua conta do Google, será solicitado que você faça login agora (se você ainda não tiver uma conta do Google, precisará criar uma). Depois de logado, ao tentar executar o código, você verá um aviso de segurança informando que este notebook não foi criado pelo Google. Uma pessoa mal-intencionada poderia criar um notebook que tenta enganá-lo para inserir suas credenciais do Google e acessar seus dados pessoais, então, antes de executar um notebook, sempre certifique-se de que confia em seu autor (ou verifique o que cada célula de código fará antes de executá-la). Supondo que você confie em mim (ou que planeja verificar cada célula de código), você pode clicar em "Executar mesmo assim".

O Colab então alocará um novo runtime para você: esta é uma máquina virtual gratuita localizada nos servidores do Google que contém várias ferramentas e bibliotecas Python, incluindo tudo o que você precisará para a maioria dos capítulos (em alguns capítulos, você precisará executar um comando para instalar bibliotecas adicionais). Isso levará alguns segundos. Em seguida, o Colab se conectará automaticamente a este runtime e o usará para executar sua nova célula de código. Importante: o código é executado no runtime, não na sua máquina. A saída do código será exibida abaixo da célula. Parabéns, você executou algum código Python no Colab!

**DICA**

Para inserir uma nova célula de código, você também pode digitar Ctrl-M (ou Cmd-M no macOS) seguido de A (para inserir acima da célula ativa) ou B (para inserir abaixo). Há muitos outros atalhos de teclado disponíveis: você pode visualizá-los e editá-los digitando Ctrl-M (ou Cmd-M) e depois H. Se você optar por executar os notebooks no Kaggle ou em sua própria máquina usando JupyterLab ou uma IDE como o Visual Studio Code com a extensão Jupyter, verá algumas pequenas diferenças—runtimes são chamados de kernels, a interface do usuário e os atalhos de teclado são ligeiramente diferentes, etc.—mas alternar entre um ambiente Jupyter e outro não é muito difícil.

## Salvando Suas Alterações de Código e Seus Dados

Você pode fazer alterações em um notebook do Colab, e elas persistirão enquanto você mantiver sua aba do navegador aberta. Mas assim que você fechá-la, as alterações serão perdidas. Para evitar isso, certifique-se de salvar uma cópia do notebook no seu Google Drive selecionando Arquivo → "Salvar uma cópia no Drive". Alternativamente, você pode baixar o notebook para o seu computador selecionando Arquivo → Download → "Download .ipynb". Então, você pode visitar _https://colab.research.google.com_ e abrir o notebook novamente (seja do Google Drive ou fazendo upload do seu computador).

---

# AVISO

O Google Colab é destinado apenas para uso interativo: você pode brincar nos notebooks e ajustar o código como quiser, mas não pode deixar os notebooks rodando desacompanhados por um longo período de tempo, ou o runtime será desligado e todos os seus dados serão perdidos.

Se o notebook gerar dados que você se importa, certifique-se de baixar esses dados antes que o runtime seja desligado. Para fazer isso, clique no ícone Arquivos (veja o passo 1 na Figura 2-5), encontre o arquivo que deseja baixar, clique nos pontos verticais ao lado dele (passo 2) e clique em Baixar (passo 3). Alternativamente, você pode montar seu Google Drive no runtime, permitindo que o notebook leia e escreva arquivos diretamente no Google Drive como se fosse um diretório local. Para isso, clique no ícone Arquivos (passo 1), depois clique no ícone do Google Drive (circulado na Figura 2-5) e siga as instruções na tela.

---

### Arquivos
- [ ] sample_data
- [ ] my_great_model
- [ ] ...

### Download
- Renomear arquivo
- Excluir arquivo
- Copiar caminho
- Atualizar

---

**Figura 2-5.** Baixando um arquivo de um runtime do Google Colab (passos 1 a 3), ou montando seu Google Drive (ícone circulado)

Por padrão, seu Google Drive será montado em /content/drive/MyDrive. Se você quiser fazer backup de um arquivo de dados, simplesmente copie-o para este diretório executando !cp /content/my_great_model /content/drive/MyDrive. Qualquer comando que comece com um ponto de exclamação (!) é tratado como um comando de shell, não como código Python: cp é o comando de shell do Linux para copiar um arquivo de um caminho para outro. Observe que os runtimes do Colab rodam no Linux (especificamente, Ubuntu).

---

# O Poder e o Perigo da Interatividade

Os notebooks Jupyter são interativos, e isso é ótimo: você pode executar cada célula uma por uma, parar a qualquer momento, inserir uma célula, brincar com o código, voltar e executar a mesma célula novamente, etc., e eu recomendo fortemente que você faça isso. Se você apenas executar as células uma por uma sem nunca brincar com elas, você não aprenderá tão rápido. No entanto, essa flexibilidade tem um preço: é muito fácil executar as células na ordem errada ou esquecer de executar uma célula. Se isso acontecer, as células de código subsequentes provavelmente falharão. Por exemplo, a primeira célula de código em cada notebook contém código de configuração (como imports), então certifique-se de executá-la primeiro, ou nada funcionará.

---

## DICA

Se você encontrar um erro estranho, tente reiniciar o runtime (selecionando Runtime → "Reiniciar runtime" no menu) e então execute todas as células novamente desde o início do notebook. Isso geralmente resolve o problema. Se não resolver, é provável que uma das alterações que você fez tenha quebrado o notebook: basta reverter para o notebook original e tentar novamente. Se ainda falhar, por favor, abra uma issue no GitHub.

---

# Código do Livro Versus Código do Notebook

Você pode notar algumas pequenas diferenças entre o código deste livro e o código nos notebooks. Isso pode acontecer por várias razões:

- Uma biblioteca pode ter mudado um pouco no momento em que você está lendo estas linhas, ou talvez, apesar de meus melhores esforços, eu tenha cometido um erro no livro. Infelizmente, não posso magicamente consertar o código na sua cópia deste livro (a menos que você esteja lendo uma cópia eletrônica e possa baixar a versão mais recente), mas eu *posso* consertar os notebooks. Então, se você encontrar um erro depois de copiar o código deste livro, por favor, procure o código corrigido nos notebooks: eu me esforçarei para mantê-los livres de erros e atualizados com as versões mais recentes das bibliotecas.

- Os notebooks contêm algum código extra para embelezar as figuras (adicionar rótulos, definir tamanhos de fonte, etc.) e salvá-las em alta resolução para este livro. Você pode ignorar com segurança este código extra se quiser.

Eu otimizei o código para legibilidade e simplicidade: fiz o mais linear e plano possível, definindo muito poucas funções ou classes. O objetivo é garantir que o código que você está executando esteja geralmente na sua frente, e não aninhado dentro de várias camadas de abstrações que você precisa procurar. Isso também facilita para você brincar com o código. Para simplificar, há um tratamento de erros limitado, e coloquei alguns dos imports menos comuns exatamente onde são necessários (em vez de colocá-los no topo do arquivo, como é recomendado pelo guia de estilo PEP 8 do Python). Dito isso, seu código de produção não será muito diferente: apenas um pouco mais modular e com testes e tratamento de erros adicionais.

OK! Quando você estiver confortável com o Colab, estará pronto para baixar os dados.

# Baixar os Dados

Em ambientes típicos, seus dados estariam disponíveis em um banco de dados relacional ou em algum outro armazenamento de dados comum, e espalhados por várias tabelas/documentos/arquivos. Para acessá-los, você precisaria primeiro obter suas credenciais e autorizações de acesso[4] e se familiarizar com o esquema dos dados. Neste projeto, no entanto, as coisas são muito mais simples: você apenas baixará um único arquivo compactado, _housing.tgz_, que contém um arquivo de valores separados por vírgulas (CSV) chamado _housing.csv_ com todos os dados.

Em vez de baixar e descompactar os dados manualmente, geralmente é preferível escrever uma função que faça isso por você. Isso é útil principalmente se os dados mudarem regularmente: você pode escrever um pequeno script que usa a função para buscar os dados mais recentes (ou você pode configurar um trabalho agendado para fazer isso automaticamente em intervalos regulares). Automatizar o processo de busca dos dados também é útil se você precisar instalar o conjunto de dados em várias máquinas.

Aqui está a função para buscar e carregar os dados:

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

Quando load_housing_data() é chamada, ela procura pelo arquivo _datasets/housing.tgz_. Se não o encontrar, cria o diretório _datasets_ dentro do diretório atual (que é /content por padrão, no Colab), baixa o arquivo _housing.tgz_ do repositório GitHub _ageron/data_ e extrai seu conteúdo no diretório _datasets_; isso cria o diretório _datasets/housing_ com o arquivo _housing.csv_ dentro dele. Por fim, a função carrega este arquivo CSV em um objeto DataFrame do Pandas contendo todos os dados e o retorna.

---

# Dar uma Olhada Rápida na Estrutura dos Dados

Você começa olhando as cinco primeiras linhas de dados usando o método head() do DataFrame (veja a Figura 2-6).

---

**longitude latitude housing_median_age median_income ocean_proximity median_house_value**

|    |    |    |    |    |
|---|---|---|---|---|
| 0   | -122.23    | 37.88    | 41.0    | 8.3252    | NEAR BAY    | 452600.0    |
| 1   | -122.22    | 37.86    | 21.0    | 8.3014    | NEAR BAY    | 358500.0    |
| 2   | -122.24    | 37.85    | 52.0    | 7.2574    | NEAR BAY    | 352100.0    |
| 3   | -122.25    | 37.85    | 52.0    | 5.6431    | NEAR BAY    | 341300.0    |
| 4   | -122.25    | 37.85    | 52.0    | 3.8462    | NEAR BAY    | 342200.0    |

*Figura 2-6. As cinco primeiras linhas no conjunto de dados*

Cada linha representa um distrito. Há 10 atributos (nem todos são mostrados na captura de tela): longitude, latitude, housing_median_age, total_rooms, total_bedrooms, population, households, median_income, median_house_value e ocean_proximity.

O método info() é útil para obter uma descrição rápida dos dados, em particular o número total de linhas, o tipo de cada atributo e o número de valores não nulos:

>>> housing.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 20640 entries, 0 to 20639
Data columns (total 10 columns):
# Column    Non-Null Count    Dtype
--- --- --- ---
0    longitude    20640 non-null    float64
1    latitude    20640 non-null    float64
2    housing_median_age    20640 non-null    float64
3    total_rooms    20640 non-null    float64
4    total_bedrooms    20433 non-null    float64
5    population    20640 non-null    float64
6    households    20640 non-null    float64
7    median_income    20640 non-null    float64
8    median_house_value    20640 non-null    float64
9    ocean_proximity    20640 non-null    object
dtypes: float64(9), object(1)
memory usage: 1.6+ MB

---

# NOTA

Neste livro, quando um exemplo de código contém uma mistura de código e saídas, como é o caso aqui, ele é formatado como no interpretador Python, para melhor legibilidade: as linhas de código são prefixadas com >>> (ou ... para blocos recuados), e as saídas não têm prefixo.

Há 20.640 instâncias no conjunto de dados, o que significa que ele é bastante pequeno para os padrões de machine learning, mas é perfeito para começar. Você nota que o atributo total_bedrooms tem apenas 20.433 valores não nulos, o que significa que 207 distritos estão faltando esse recurso. Você precisará cuidar disso mais tarde.

Todos os atributos são numéricos, exceto ocean_proximity. Seu tipo é object, então ele pode conter qualquer tipo de objeto Python. Mas como você carregou esses dados de um arquivo CSV, sabe que deve ser um atributo de texto. Quando você olhou as cinco primeiras linhas, provavelmente notou que os valores na coluna ocean_proximity eram repetitivos, o que significa que provavelmente é um atributo categórico. Você pode descobrir quais categorias existem e quantos distritos pertencem a cada categoria usando o método value_counts():

>>> housing["ocean_proximity"].value_counts()
<1H OCEAN    9136
INLAND    6551
NEAR OCEAN    2658
NEAR BAY    2290
ISLAND    5
Name: ocean_proximity, dtype: int64

Vamos olhar para os outros campos. O método describe() mostra um resumo dos atributos numéricos (Figura 2-7).

---

hoursing.describe()

    longitude    latitude housing_median_age total_rooms total_bedrooms median_house_value
count  20640.000000  20640.000000    20640.000000  20640.000000    20433.000000    20640.000000
mean  -119.569704    35.631861    28.639486    2835.763081    537.870553    208855.816909
std    2.003532    2.135952    12.585558    2181.615252    421.385070    115395.615874
min  -124.350000    32.540000    1.000000    2.000000    1.000000    14999.000000
25%  -121.800000    33.930000    18.000000    1447.750000    298.000000    119600.000000
50%  -118.490000    34.260000    29.000000    2127.000000    435.000000    179700.000000
75%  -118.010000    37.710000    37.000000    3148.000000    647.000000    264725.000000
max  -114.310000    41.950000    52.000000    39320.000000    6445.000000    500001.000000

Figura 2-7. Resumo de cada atributo numérico

As linhas count, mean, min e max são autoexplicativas. Observe que os valores nulos são ignorados (então, por exemplo, a contagem de total_bedrooms é 20.433, não 20.640). A linha std mostra o desvio padrão, que mede o quão dispersos estão os valores.\(^5\) As linhas 25%, 50% e 75% mostram os percentis correspondentes: um percentil indica o valor abaixo do qual uma determinada porcentagem de observações em um grupo de observações cai. Por exemplo, 25% dos distritos têm um housing_median_age menor que 18, enquanto 50% são menores que 29 e 75% são menores que 37. Esses são frequentemente chamados de 25º percentil (ou primeiro quartil), mediana e 75º percentil (ou terceiro quartil).

Outra maneira rápida de ter uma noção do tipo de dados com os quais você está lidando é plotar um histograma para cada atributo numérico. Um histograma mostra o número de instâncias (no eixo vertical) que têm um determinado intervalo de valores (no eixo horizontal). Você pode plotar um atributo por vez ou chamar o método hist() em todo o conjunto de dados (como mostrado no exemplo de código a seguir), e ele plotará um histograma para cada atributo numérico (veja a Figura 2-8):

import matplotlib.pyplot as plt

housing.hist(bins=50, figsize=(12, 8))
plt.show()

---

Olhando para esses histogramas, você nota algumas coisas:

- Primeiro, o atributo de renda média não parece estar expresso em dólares americanos (USD). Após verificar com a equipe que coletou os dados, você é informado de que os dados foram escalonados e limitados em 15 (na verdade, 15.0001) para rendas médias mais altas e em 0.5 (na verdade, 0.4999) para rendas médias mais baixas. Os números representam aproximadamente dezenas de milhares de dólares (por exemplo, 3 significa cerca de \$30.000). Trabalhar com atributos pré-processados é comum em machine learning, e não é necessariamente um problema, mas você deve tentar entender como os dados foram computados.

- A idade média da habitação e o valor médio da casa também foram limitados. O último pode ser um problema sério, pois é seu atributo alvo (seus rótulos). Seus algoritmos de machine learning podem aprender que os preços nunca ultrapassam esse limite. Você precisa verificar com a equipe do cliente (a equipe que usará a saída do seu sistema) para ver se isso é um problema ou não. Se eles disserem que precisam de previsões precisas mesmo além de \$500.000, então você tem duas opções:

- Coletar rótulos adequados para os distritos cujos rótulos foram limitados.

- Remover esses distritos do conjunto de treinamento (e também do conjunto de teste, já que seu sistema não deve ser avaliado de forma ruim se prever valores além de \$500.000).

- Esses atributos têm escalas muito diferentes. Discutiremos isso mais tarde neste capítulo, quando explorarmos o escalonamento de recursos.

- Finalmente, muitos histogramas são assimétricos à direita: eles se estendem muito mais para a direita da mediana do que para a esquerda. Isso pode dificultar a detecção de padrões por alguns algoritmos de machine learning. Mais tarde, você tentará transformar esses atributos para ter distribuições mais simétricas e em forma de sino.

Agora você deve ter uma melhor compreensão do tipo de dados com os quais está lidando.

**AVISO**

Espere! Antes de olhar para os dados mais a fundo, você precisa criar um conjunto de teste, separá-lo e nunca mais olhar para ele.

## Criar um Conjunto de Teste

Pode parecer estranho separar voluntariamente parte dos dados nesta fase. Afinal, você só deu uma olhada rápida nos dados, e certamente deveria aprender muito mais sobre eles antes de decidir quais algoritmos usar, certo? Isso é verdade, mas seu cérebro é um sistema incrível de detecção de padrões, o que também significa que ele é altamente propenso a overfitting: se você olhar para o conjunto de teste, pode tropeçar em algum padrão aparentemente interessante nos dados de teste que o leva a selecionar um tipo particular de modelo de machine learning. Quando você estimar o erro de generalização usando o conjunto de teste, sua estimativa será muito otimista, e você lançará um sistema que não terá o desempenho esperado. Isso é chamado de viés de *data snooping*.

Criar um conjunto de teste é teoricamente simples; escolha algumas instâncias aleatoriamente, tipicamente 20% do conjunto de dados (ou menos se seu conjunto de dados for muito grande), e separe-as:

import numpy as np

def shuffle_and_split_data(data, test_ratio):
    shuffled_indices = np.random.permutation(len(data))
    test_set_size = int(len(data) * test_ratio)
    test_indices = shuffled_indices[:test_set_size]
    train_indices = shuffled_indices[test_set_size:]
    return data.iloc[train_indices], data.iloc[test_indices]

Você pode então usar esta função assim:

>>> train_set, test_set = shuffle_and_split_data(housing, 0.2)
>>> len(train_set)
16512
>>> len(test_set)
4128

Bem, isso funciona, mas não é perfeito: se você executar o programa novamente, ele gerará um conjunto de teste diferente! Com o tempo, você (ou seus algoritmos de machine learning) verá todo o conjunto de dados, o que é exatamente o que você quer evitar.

Uma solução é salvar o conjunto de teste na primeira execução e depois carregá-lo nas execuções subsequentes. Outra opção é definir a semente do gerador de números aleatórios (por exemplo, com np.random.seed(42))\(^6\) antes de chamar np.random.permutation() para que ele sempre gere os mesmos índices embaralhados.

No entanto, ambas as soluções quebrarão na próxima vez que você buscar um conjunto de dados atualizado. Para ter uma divisão estável entre treino/teste mesmo após atualizar o conjunto de dados, uma solução comum é usar o identificador de cada instância para decidir se ela deve ir para o conjunto de teste (assumindo que as instâncias têm identificadores únicos e imutáveis). Por exemplo, você poderia calcular um hash do identificador de cada instância e colocar essa instância no conjunto de teste se o hash for menor ou igual a 20% do valor máximo de hash. Isso garante que o conjunto de teste permanecerá consistente em várias execuções, mesmo que você atualize o conjunto de dados. O novo conjunto de teste conterá 20% das novas instâncias, mas não conterá nenhuma instância que estava anteriormente no conjunto de treinamento.

Aqui está uma possível implementação:

from 2lib import crc32

def is_id_in_test_set(identifier, test_ratio):
    return crc32(np.int64(identifier)) < test_ratio * 2**32

def split_data_with_id_hash(data, test_ratio, id_column):
    tds = data[id_column]
    in_test_set = tds.apply(lambda td: is_id_in_test_set(id_, test_ratio))
    return data.loc[-in_test_set], data.loc[in_test_set]

Infelizmente, o conjunto de dados de habitação não tem uma coluna de identificador. A solução mais simples é usar o índice da linha como ID:

housing_with_id = housing.reset_index()  # adiciona uma coluna `index`
train_set, test_set = split_data_with_id_hash(housing_with_id, 0.2, "index")

Se você usar o índice da linha como um identificador único, precisa garantir que novos dados sejam anexados ao final do conjunto de dados e que nenhuma linha seja excluída. Se isso não for possível, você pode tentar usar os recursos mais estáveis para construir um identificador único. Por exemplo, a latitude e a longitude de um distrito são garantidamente estáveis por alguns milhões de anos, então você poderia combiná-las em um ID assim:[7]

housing_with_id["id"] = housing["longitude"] * 1000 + housing["latitude"]
train_set, test_set = split_data_with_id_hash(housing_with_id, 0.2, "id")

O Scikit-Learn fornece algumas funções para dividir conjuntos de dados em vários subconjuntos de várias maneiras. A função mais simples é train_test_split(), que faz basicamente a mesma coisa que a função shuffle_and_split_data() que definimos anteriormente, com alguns recursos adicionais. Primeiro, há um parâmetro random_state que permite definir a semente do gerador aleatório. Segundo, você pode passar vários conjuntos de dados com o mesmo número de linhas, e ele os dividirá nos mesmos índices (isso é muito útil, por exemplo, se você tiver um DataFrame separado para rótulos):

from sklearn.model_selection import train_test_split

train_set, test_set = train_test_split(housing, test_size=0.2,
random_state=42)

Até agora, consideramos métodos de amostragem puramente aleatórios. Isso geralmente é bom se seu conjunto de dados for grande o suficiente (especialmente em relação ao número de atributos), mas se não for, você corre o risco de introduzir um viés de amostragem significativo. Quando os funcionários de uma empresa de pesquisa decidem ligar para 1.000 pessoas para fazer algumas perguntas, eles não escolhem 1.000 pessoas aleatoriamente em uma lista telefônica. Eles tentam garantir que essas 1.000 pessoas sejam representativas de toda a população, em relação às perguntas que querem fazer. Por exemplo, a população dos EUA é 51,1% feminina e 48,9% masculina, então uma pesquisa bem conduzida nos EUA tentaria manter essa proporção na amostra: 511 mulheres e 489 homens (pelo menos se parecer possível que as respostas variem entre gêneros). Isso é chamado de *amostragem estratificada*: a população é dividida em subgrupos homogêneos chamados estratos, e o número certo de instâncias é amostrado de cada estrato para garantir que o conjunto de teste seja representativo da população geral. Se as pessoas que conduzem a pesquisa usassem amostragem puramente aleatória, haveria cerca de 10,7% de chance de amostrar um conjunto de teste distorcido com menos de 48,5% de participantes do sexo feminino ou mais de 53,5%. De qualquer forma, os resultados da pesquisa provavelmente seriam bastante tendenciosos.

Suponha que você tenha conversado com alguns especialistas que disseram que a renda média é um atributo muito importante para prever os preços médios das casas. Você pode querer garantir que o conjunto de teste seja representativo das várias categorias de renda em todo o conjunto de dados. Como a renda média é um atributo numérico contínuo, você primeiro precisa criar um atributo de categoria de renda. Vamos olhar mais de perto o histograma de renda média (de volta à Figura 2-8): a maioria dos valores de renda média está agrupada em torno de 1,5 a 6 (ou seja, \$15.000-\$60.000), mas algumas rendas médias vão muito além de 6. É importante ter um número suficiente de instâncias em seu conjunto de dados para cada estrato, ou a estimativa da importância de um estrato pode ser tendenciosa. Isso significa que você não deve ter muitos estratos, e cada estrato deve ser grande o suficiente. O código a seguir usa a função pd.cut() para criar um atributo de categoria de renda com cinco categorias (rotuladas de 1 a 5); a categoria 1 varia de 0 a 1,5 (ou seja, menos de \$15.000), a categoria 2 de 1,5 a 3, e assim por diante:

housing["income_cat"] = pd.cut(housing["median_income"], bins=[0., 1.5, 3.0, 4.5, 6., np.inf], labels=[1, 2, 3, 4, 5])

Essas categorias de renda são representadas na Figura 2-9:

housing["income_cat"].value_counts().sort_index().plot.bar(rot=0, grid=True) plt.xlabel("Categoria de Renda") plt.ylabel("Número de Distritos") plt.show()

Agora você está pronto para fazer uma amostragem estratificada com base na categoria de renda. O Scikit-Learn fornece várias classes de divisão no pacote sklearn.model_selection que implementam várias estratégias para dividir seu conjunto de dados em um conjunto de treinamento e um conjunto de teste. Cada divisor tem um método split() que retorna um iterador sobre diferentes divisões de treino/teste dos mesmos dados.

---

Para ser preciso, o método split() gera os índices de treinamento e teste, não os dados em si. Ter várias divisões pode ser útil se você quiser estimar melhor o desempenho do seu modelo, como veremos quando discutirmos validação cruzada mais adiante neste capítulo. Por exemplo, o código a seguir gera 10 divisões estratificadas diferentes do mesmo conjunto de dados:

from sklearn.model_selection import StratifiedShuffleSplit

splitter = StratifiedShuffleSplit(n_splits=10, test_size=0.2, random_state=42) strat_splits = []
for train_index, test_index in splitter.split(housing, housing["income_cat"]): strat_train_set_n = housing.1loc[train_index] strat_test_set_n = housing.1loc[test_index] strat_splits.append([strat_train_set_n, strat_test_set_n])

Por enquanto, você pode apenas usar a primeira divisão:

strat_train_set, strat_test_set = strat_splits[0]

Ou, como a amostragem estratificada é bastante comum, há uma maneira mais curta de obter uma única divisão usando a função train_test_split() com o argumento stratify:

strat_train_set, strat_test_set = train_test_split( housing, test_size=0.2, stratify=housing["income_cat"], random_state=42)

---

Vamos ver se isso funcionou como esperado. Você pode começar olhando as proporções das categorias de renda no conjunto de teste:

>>> strat_test_set["income_cat"].value_counts() / len(strat_test_set)
3    0.350533
2    0.318798
4    0.176357
5    0.114341
1    0.039971

Name: income_cat, dtype: float64

Com código semelhante, você pode medir as proporções das categorias de renda no conjunto de dados completo. A Figura 2-10 compara as proporções das categorias de renda no conjunto de dados geral, no conjunto de teste gerado com amostragem estratificada e em um conjunto de teste gerado usando amostragem puramente aleatória. Como você pode ver, o conjunto de teste gerado usando amostragem estratificada tem proporções de categorias de renda quase idênticas às do conjunto de dados completo, enquanto o conjunto de teste gerado usando amostragem puramente aleatória é distorcido.

| Categoria de Renda | % Geral | % Estratificada | % Aleatória | Erro Estrat. % | Erro Aleat. % |
|---|---|---|---|---|---|
|    | 1    | 3.98    | 4.00    | 4.24    | 0.36    | 6.45    |
|    | 2    | 31.88    | 31.88    | 30.74    | -0.02    | -3.59    |
|    | 3    | 35.06    | 35.05    | 34.52    | -0.01    | -1.53    |
|    | 4    | 17.63    | 17.64    | 18.41    | 0.03    | 4.42    |
|    | 5    | 11.44    | 11.43    | 12.09    | -0.08    | 5.63    |

Figura 2-10. Comparação de viés de amostragem entre amostragem estratificada e puramente aleatória

Você não usará a coluna income_cat novamente, então pode descartá-la, revertendo os dados de volta ao estado original:

for set_in (strat_train_set, strat_test_set):
    set_drop("income_cat", axis=1, inplace=True)

Passamos bastante tempo na geração do conjunto de teste por uma boa razão: esta é uma parte frequentemente negligenciada, mas crítica, de um projeto de machine learning. Além disso, muitas dessas ideias serão úteis mais tarde, quando discutirmos validação cruzada. Agora é hora de passar para a próxima etapa: explorar os dados.

---

# Explorar e Visualizar os Dados para Obter Insights

Até agora, você só deu uma olhada rápida nos dados para ter uma compreensão geral do tipo de dados que está manipulando. Agora, o objetivo é ir um pouco mais a fundo.

Primeiro, certifique-se de que você separou o conjunto de teste e está apenas explorando o conjunto de treinamento. Além disso, se o conjunto de treinamento for muito grande, você pode querer amostrar um conjunto de exploração, para tornar as manipulações fáceis e rápidas durante a fase de exploração. Neste caso, o conjunto de treinamento é bastante pequeno, então você pode trabalhar diretamente com o conjunto completo. Como você vai experimentar várias transformações do conjunto de treinamento completo, deve fazer uma cópia do original para poder voltar a ele depois:

\[ \text{housing = strat_train_set.copy()} \]

## Visualizando Dados Geográficos

Como o conjunto de dados inclui informações geográficas (latitude e longitude), é uma boa ideia criar um gráfico de dispersão de todos os distritos para visualizar os dados (Figura 2-11):

\[ \text{housing.plot(kind="scatter", x="longitude", y="latitude", grid=True)} \]
\[ \text{plt.show()} \]

---

Isso parece com a Califórnia, mas além disso é difícil ver qualquer padrão específico. Definir a opção alpha para 0,2 torna muito mais fácil visualizar os lugares onde há uma alta densidade de pontos de dados (Figura 2-12):

housing.plot(kind="scatter", x="longitude", y="latitude", grid=True, alpha=0.2) plt.show()

Agora está muito melhor: você pode ver claramente as áreas de alta densidade, ou seja, a Bay Area e ao redor de Los Angeles e San Diego, além de uma longa linha de áreas de densidade bastante alta no Vale Central (em particular, ao redor de Sacramento e Fresno).

Nossos cérebros são muito bons em detectar padrões em imagens, mas você pode precisar brincar com os parâmetros de visualização para destacar os padrões.

---

_Figura 2-12. Uma melhor visualização que destaca áreas de alta densidade_

Em seguida, você olha para os preços das casas (Figura 2-13). O raio de cada círculo representa a população do distrito (opção s), e a cor representa o preço (opção c). Aqui você usa um mapa de cores predefinido (opção cmap) chamado jet, que varia de azul (valores baixos) a vermelho (preços altos):[8]

housing.plot(kind="scatter", x="longitude", y="latitude", grid=True,
    s=housing["population"] / 100, label="population",
    c="median_house_value", cmap="jet", colorbar=True,
    legend=True, sharex=False, figsize=(10, 7))

plt.show()

Esta imagem diz que os preços das casas estão muito relacionados à localização (por exemplo, perto do oceano) e à densidade populacional, como você provavelmente já sabia. Um algoritmo de clustering deve ser útil para detectar o cluster principal e para adicionar novos recursos que medem a proximidade com os centros dos clusters. O atributo de proximidade do oceano também pode ser útil, embora no norte da Califórnia os preços das casas em distritos costeiros não sejam muito altos, então não é uma regra simples.

---

# Procurar por Correlações

Como o conjunto de dados não é muito grande, você pode facilmente calcular o *coeficiente de correlação padrão* (também chamado de *r de Pearson*) entre cada par de atributos usando o método corr():

corr_matrix = housing.corr()

Agora você pode ver o quanto cada atributo está correlacionado com o valor médio da casa:

>>> corr_matrix["median_house_value"].sort_values(ascending=False)
    median_house_value    1.000000
    median_income    0.688380
    total_rooms    0.137455
    housing_median_age    0.102175
    households    0.071426

---

total_bedrooms    0.054635
population    -0.020153
longitude    -0.050859
latitude    -0.139584
Name: median_house_value, dtype: float64

O coeficiente de correlação varia de \(-1\) a \(1\). Quando está próximo de \(1\), significa que há uma forte correlação positiva; por exemplo, o valor médio da casa tende a subir quando a renda média sobe. Quando o coeficiente está próximo de \(-1\), significa que há uma forte correlação negativa; você pode ver uma pequena correlação negativa entre a latitude e o valor médio da casa (ou seja, os preços têm uma ligeira tendência a cair quando você vai para o norte). Finalmente, coeficientes próximos de 0 significam que não há correlação linear.

Outra maneira de verificar a correlação entre atributos é usar a função scatter_matrix() do Pandas, que plota cada atributo numérico contra todos os outros atributos numéricos. Como agora há 11 atributos numéricos, você obteria \(11^2 = 121\) gráficos, o que não caberia em uma página—então você decide se concentrar em alguns atributos promissores que parecem mais correlacionados com o valor médio da casa (Figura 2-14):

from pandas.plotting import scatter_matrix

attributes = ["median_house_value", "median_income", "total_rooms",
    "housing_median_age"]
scatter_matrix(housing[attributes], f!gsize=(12, 8))
plt.show()

---

A diagonal principal estaria cheia de linhas retas se o Pandas plotasse cada variável contra si mesma, o que não seria muito útil. Então, em vez disso, o Pandas exibe um histograma de cada atributo (outras opções estão disponíveis; consulte a documentação do Pandas para mais detalhes).

Olhando para os gráficos de dispersão de correlação, parece que o atributo mais promissor para prever o valor médio da casa é a renda média, então você amplia o gráfico de dispersão deles (Figura 2-15):

housing.plot(kind="scatter", x="median_income", y="median_house_value", alpha=0.1, grid=True)

plt.show()

Figura 2-14: Esta matriz de dispersão plota cada atributo numérico contra todos os outros atributos numéricos, além de um histograma dos valores de cada atributo numérico na diagonal principal (de cima à esquerda para baixo à direita)

---

Este gráfico revela algumas coisas. Primeiro, a correlação é realmente bastante forte; você pode ver claramente a tendência de alta, e os pontos não estão muito dispersos. Segundo, o limite de preço que você notou anteriormente é claramente visível como uma linha horizontal em \$500.000. Mas o gráfico também revela outras linhas retas menos óbvias: uma linha horizontal em torno de \$450.000, outra em torno de \$350.000, talvez uma em torno de \$280.000, e algumas mais abaixo. Você pode querer tentar remover os distritos correspondentes para evitar que seus algoritmos aprendam a reproduzir essas peculiaridades dos dados.

---

# AVISO

O coeficiente de correlação só mede correlações lineares ("à medida que x aumenta, y geralmente aumenta/diminui"). Ele pode perder completamente relações não lineares (por exemplo, "à medida que x se aproxima de 0, y geralmente aumenta"). A Figura 2-16 mostra uma variedade de conjuntos de dados junto com seus coeficientes de correlação. Observe como todos os gráficos da linha inferior têm um coeficiente de correlação igual a 0, apesar do fato de que seus eixos claramente não são independentes: esses são exemplos de relações não lineares. Além disso, a segunda linha mostra exemplos onde o coeficiente de correlação é igual a 1 ou –1; observe que isso não tem nada a ver com a inclinação. Por exemplo, sua altura em polegadas tem um coeficiente de correlação de 1 com sua altura em pés ou em nanômetros.

\[\begin{array}{cccccc}
1 & 0.8 & 0.4 & 0 & -0.4 & -0.8 & -1 \\
1 & 1 & 1 & -1 & -1 & -1 & -1 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 \\
\end{array}\]

*Figura 2-16. Coeficiente de correlação padrão de vários conjuntos de dados (fonte: Wikipedia; imagem de domínio público)*

## Experimentar com Combinações de Atributos

Espero que as seções anteriores tenham dado uma ideia de algumas maneiras de explorar os dados e obter insights. Você identificou algumas peculiaridades nos dados que podem precisar ser limpas antes de alimentar os dados em um algoritmo de machine learning, e encontrou correlações interessantes entre os atributos, em particular com o atributo alvo. Você também notou que alguns atributos têm uma distribuição assimétrica à direita, então pode querer transformá-los (por exemplo, calculando seu logaritmo ou raiz quadrada). Claro, seus resultados variarão consideravelmente com cada projeto, mas as ideias gerais são semelhantes.

---

Uma última coisa que você pode querer fazer antes de preparar os dados para algoritmos de machine learning é tentar várias combinações de atributos. Por exemplo, o número total de quartos em um distrito não é muito útil se você não souber quantas famílias existem. O que você realmente quer é o número de quartos por família. Da mesma forma, o número total de quartos por si só não é muito útil: você provavelmente quer compará-lo ao número de quartos. E a população por família também parece ser uma combinação interessante de atributos para examinar. Você cria esses novos atributos da seguinte forma:

housing["rooms_per_house"] = housing["total_rooms"] / housing["households"]
housing["bedrooms_ratio"] = housing["total_bedrooms"] / housing["total_rooms"]
housing["people_per_house"] = housing["population"] / housing["households"]

E então você olha para a matriz de correlação novamente:

>>> corr_matrix = housing.corr()
>>> corr_matrix["median_house_value"].sort_values(ascending=False)
median_house_value    1.000000
median_income    0.688380
rooms_per_house    0.143663
total_rooms    0.137455
housing_median_age    0.102175
households    0.071426
total_bedrooms    0.054635
population    -0.020153
people_per_house  -0.038224
longitude    -0.050859
latitude    -0.139584
bedrooms_ratio    -0.256397
Name: median_house_value, dtype: float64

Ei, nada mal! O novo atributo bedrooms_ratio está muito mais correlacionado com o valor médio da casa do que o número total de quartos ou quartos. Aparentemente, casas com uma proporção menor de quartos por quarto tendem a ser mais caras. O número de quartos por família também é mais informativo do que o número total de quartos em um distrito—obviamente, quanto maiores as casas, mais caras elas são.

Esta rodada de exploração não precisa ser absolutamente completa; o ponto é começar com o pé direito e obter rapidamente insights que o ajudarão a obter um primeiro protótipo razoavelmente bom. Mas este é um processo iterativo: uma vez que você tiver um protótipo funcionando, você pode analisar sua saída para obter mais insights e voltar a esta etapa de exploração.

---

# Preparar os Dados para Algoritmos de Machine Learning

É hora de preparar os dados para seus algoritmos de machine learning. Em vez de fazer isso manualmente, você deve escrever funções para esse propósito, por várias boas razões:

- Isso permitirá que você reproduza essas transformações facilmente em qualquer conjunto de dados (por exemplo, da próxima vez que você obtiver um novo conjunto de dados).

- Você gradualmente construirá uma biblioteca de funções de transformação que poderá reutilizar em projetos futuros.

- Você poderá usar essas funções em seu sistema ao vivo para transformar novos dados antes de alimentá-los em seus algoritmos.

- Isso tornará possível tentar várias transformações e ver qual combinação de transformações funciona melhor.

Mas primeiro, volte a um conjunto de treinamento limpo (copiando strat_train_set mais uma vez). Você também deve separar os preditores e os rótulos, já que você não necessariamente quer aplicar as mesmas transformações aos preditores e aos valores alvo (note que drop() cria uma cópia dos dados e não afeta strat_train_set):

housing = strat_train_set.drop("median_house_value", axis=1)
housing_labels = strat_train_set["median_house_value"].copy()

## Limpar os Dados

A maioria dos algoritmos de machine learning não consegue trabalhar com recursos ausentes, então você precisará cuidar disso. Por exemplo, você notou anteriormente que o atributo total_bedrooms tem alguns valores ausentes. Você tem três opções para corrigir isso:

1. Descarte os distritos correspondentes.

2. Descarte todo o atributo.

3. Defina os valores ausentes para algum valor (zero, a média, a mediana, etc.). Isso é chamado de *imputação*.

Você pode realizar isso facilmente usando os métodos dropna(), drop() e fillna() do DataFrame do Pandas:

housing.dropna(subset=["total_bedrooms"], inplace=True)  # opção 1

housing.drop("total_bedrooms", axis=1)  # opção 2

median = housing["total_bedrooms"].median()  # opção 3
housing["total_bedrooms"].fillna(median, inplace=True)

Você decide optar pela opção 3, pois é a menos destrutiva, mas em vez do código anterior, você usará uma classe útil do Scikit-Learn: SimpleImputer. A vantagem é que ela armazenará o valor mediano de cada recurso: isso tornará possível imputar valores ausentes não apenas no conjunto de treinamento, mas também no conjunto de validação, no conjunto de teste e em quaisquer novos dados alimentados ao modelo. Para usá-la, primeiro você precisa criar uma instância de SimpleImputer, especificando que deseja substituir os valores ausentes de cada atributo pela mediana desse atributo:

from sklearn.inpute import SimpleImputer

imputer = SimpleImputer(strategy="median")

Como a mediana só pode ser calculada em atributos numéricos, você então precisa criar uma cópia dos dados com apenas os atributos numéricos (isso excluirá o atributo de texto ocean_proximity):

housing_num = housing.select_dtypes(include=[np.number])

---

Agora você pode ajustar a instância `imputer` aos dados de treinamento usando o método fit():
    imputer.fit(housing_num)

O `imputer` simplesmente calculou a mediana de cada atributo e armazenou o resultado em sua variável de instância `statistics_`. Apenas o atributo total_bedrooms tinha valores ausentes, mas você não pode ter certeza de que não haverá valores ausentes em novos dados depois que o sistema entrar em produção, então é mais seguro aplicar o `imputer` a todos os atributos numéricos:

>>> imputer.statistics_array([-118.51 , 34.26 , 29. , 2125. , 434. , 1167. , 408. , 3.5385])
>>> housing_num.median().values array([-118.51 , 34.26 , 29. , 2125. , 434. , 1167. , 408. , 3.5385])

Agora você pode usar este `imputer` "treinado" para transformar o conjunto de treinamento substituindo os valores ausentes pelas medianas aprendidas:

X = imputer.transform(housing_num)

Valores ausentes também podem ser substituídos pelo valor médio (strategy="mean"), pelo valor mais frequente (strategy="most_frequent"), ou por um valor constante (strategy="constant", fill_value=...). As duas últimas estratégias suportam dados não numéricos.

---

# DICA

Há também imputadores mais poderosos disponíveis no pacote sklearn.impute (apenas para recursos numéricos):

- **KNNImputer** substitui cada valor ausente pela média dos valores dos \(k\)-vizinhos mais próximos para esse recurso. A distância é baseada em todos os recursos disponíveis.

- **IterativeImputer** treina um modelo de regressão por recurso para prever os valores ausentes com base em todos os outros recursos disponíveis. Ele então treina o modelo novamente nos dados atualizados e repete o processo várias vezes, melhorando os modelos e os valores de substituição a cada iteração.

---

# DESIGN DO SCIKIT-LEARN

A API do Scikit-Learn é notavelmente bem projetada. Aqui estão os principais princípios de design:\(^9\)

**Consistência**

Todos os objetos compartilham uma interface consistente e simples:

**Estimadores**

Qualquer objeto que pode estimar alguns parâmetros com base em um conjunto de dados é chamado de estimador (por exemplo, um SimpleImputer é um estimador). A estimativa em si é realizada pelo método fit(), e ele recebe um conjunto de dados como parâmetro, ou dois para algoritmos de aprendizado supervisionado—o segundo conjunto de dados contém os rótulos. Qualquer outro parâmetro necessário para guiar o processo de estimativa é considerado um hiperparâmetro (como a estratégia de um SimpleImputer), e deve ser definido como uma variável de instância (geralmente via um parâmetro do construtor).

**Transformadores**

Alguns estimadores (como um SimpleImputer) também podem transformar um conjunto de dados; esses são chamados de transformadores. Mais uma vez, a API é simples: a transformação é realizada pelo método transform() com o conjunto de dados a ser transformado como parâmetro. Ele retorna o conjunto de dados transformado. Essa transformação geralmente depende dos parâmetros aprendidos, como é o caso de um SimpleImputer. Todos os transformadores também têm um método conveniente chamado fit_transform(), que é equivalente a chamar fit() e depois transform() (mas às vezes fit_transform() é otimizado e executa muito mais rápido).

**Preditores**

Finalmente, alguns estimadores, dado um conjunto de dados, são capazes de fazer previsões; eles são chamados de preditores. Por exemplo, o modelo LinearRegression do capítulo anterior era um preditor: dado o PIB per capita de um país, ele previu a satisfação com a vida. Um preditor tem um método predict() que recebe um conjunto de dados de novas instâncias e retorna um conjunto de dados de previsões correspondentes. Ele também tem um método score() que mede a qualidade das previsões, dado um conjunto de teste (e os rótulos correspondentes, no caso de algoritmos de aprendizado supervisionado).\(^{10}\)

**Inspeção**

Todos os hiperparâmetros do estimador são acessíveis diretamente via variáveis de instância públicas (por exemplo, imputer.strategy), e todos os parâmetros aprendidos do estimador são acessíveis via variáveis de instância públicas com um sufixo de sublinhado (por exemplo, imputer.statistics_).

**Não proliferação de classes**

Conjuntos de dados são representados como arrays NumPy ou matrizes esparsas SciPy, em vez de classes caseiras. Hiperparâmetros são apenas strings ou números Python regulares.

**Composição**

Blocos de construção existentes são reutilizados tanto quanto possível. Por exemplo, é fácil criar um estimador Pipeline a partir de uma sequência arbitrária de transformadores seguida por um estimador final, como você verá.

**Padrões sensíveis**

O Scikit-Learn fornece valores padrão razoáveis para a maioria dos parâmetros, facilitando a criação rápida de um sistema de trabalho básico.

---

Os transformadores do Scikit-Learn geram arrays NumPy (ou às vezes matrizes esparsas SciPy) mesmo quando são alimentados com DataFrames do Pandas como entrada.\(^{11}\) Então, a saída de `imputer.transform(housing_num)` é um array NumPy: X não tem nomes de colunas nem índice. Felizmente, não é muito difícil envolver X em um DataFrame e recuperar os nomes das colunas e o índice de `housing_num`:

\[ housing\_tr = pd.DataFrame(X, columns=housing_num.Columns, index=housing_num.index) \]

## Lidando com Atributos de Texto e Categóricos

Até agora, lidamos apenas com atributos numéricos, mas seus dados também podem conter atributos de texto. Neste conjunto de dados, há apenas um: o atributo `ocean_proximity`. Vamos olhar para seus valores nas primeiras instâncias:

>> housing_cat = housing[["ocean_proximity"]]
>> housing_cat.head(8)
    ocean_proximity
13096    NEAR BAY
14973    <1H OCEAN
3785    INLAND
14689    INLAND
20507    NEAR OCEAN
1286    INLAND
18078    <1H OCEAN
4396    NEAR BAY

Não é texto arbitrário: há um número limitado de valores possíveis, cada um representando uma categoria. Então, este atributo é um atributo categórico. A maioria dos algoritmos de machine learning prefere trabalhar com números, então vamos converter essas categorias de texto para números. Para isso, podemos usar a classe `OrdinalEncoder` do Scikit-Learn:

from sklearn.preprocessing import OrdinalEncoder

ordinal_encoder = OrdinalEncoder()
housing_cat_encoded = ordinal_encoder.fit_transform(housing_cat)

---

Aqui estão os primeiros valores codificados em housing_cat_encoded:

>>> housing_cat_encoded[:8] array([[3.], [0.], [1.], [1.], [4.], [1.], [0.], [3.]])

Você pode obter a lista de categorias usando a variável de instância categories_. É uma lista contendo um array 1D de categorias para cada atributo categórico (neste caso, uma lista contendo um único array, já que há apenas um atributo categórico):

>>> ordinal_encoder.categories_ [array(['<IH OCEAN', 'INLAND', 'ISLAND', 'NEAR BAY', 'NEAR OCEAN'], dtype=object)]

Um problema com essa representação é que os algoritmos de ML assumirão que dois valores próximos são mais semelhantes do que dois valores distantes. Isso pode ser bom em alguns casos (por exemplo, para categorias ordenadas como "ruim", "médio", "bom" e "excelente"), mas obviamente não é o caso para a coluna ocean_proximity (por exemplo, as categorias 0 e 4 são claramente mais semelhantes do que as categorias 0 e 1). Para corrigir esse problema, uma solução comum é criar um atributo binário por categoria: um atributo igual a 1 quando a categoria é "<1H OCEAN" (e 0 caso contrário), outro atributo igual a 1 quando a categoria é "INLAND" (e 0 caso contrário), e assim por diante. Isso é chamado de *codificação one-hot*, porque apenas um atributo será igual a 1 (quente), enquanto os outros serão 0 (frios). Os novos atributos às vezes são chamados de atributos *dummy*. O Scikit-Learn fornece uma classe OneHotEncoder para converter valores categóricos em vetores one-hot:

from sklearn.preprocessing import OneHotEncoder

---

cat_encoder = OneHotEncoder()
housing_cat_1hot = cat_encoder.fit_transform(housing_cat)

Por padrão, a saída de um OneHotEncoder é uma matriz esparsa SciPy, em vez de um array NumPy:

>>> housing_cat_1hot
<16512x5 sparse matrix of type '<class 'numpy.float64'>'
with 16512 stored elements in Compressed Sparse Row format>

Uma matriz esparsa é uma representação muito eficiente para matrizes que contêm principalmente zeros. De fato, internamente ela só armazena os valores diferentes de zero e suas posições. Quando um atributo categórico tem centenas ou milhares de categorias, codificá-lo em one-hot resulta em uma matriz muito grande cheia de 0s, exceto por um único 1 por linha. Nesse caso, uma matriz esparsa é exatamente o que você precisa: ela economizará muita memória e acelerará os cálculos. Você pode usar uma matriz esparsa principalmente como um array 2D normal,[12] mas se quiser convertê-la para um array NumPy (denso), basta chamar o método toarray():

>>> housing_cat_1hot.toarray()
array([[0., 0., 0., 1., 0.],
    [1., 0., 0., 0., 0.],
    [0., 1., 0., 0., 0.],
    ...
    [0., 0., 0., 0., 1.],
    [1., 0., 0., 0., 0.],
    [0., 0., 0., 0., 1.]])

Alternativamente, você pode definir sparse=False ao criar o OneHotEncoder, caso em que o método transform() retornará um array NumPy regular (denso) diretamente.

Assim como o OrdinalEncoder, você pode obter a lista de categorias usando a variável de instância categories_ do codificador:

>>> cat_encoder.categories_
[array(['<IH OCEAN', 'INLAND', 'ISLAND', 'NEAR BAY', 'NEAR OCEAN'],
    dtype=object)]

---

O Pandas tem uma função chamada get_dummies(), que também converte cada recurso categórico em uma representação one-hot, com um recurso binário por categoria:

>>> df_test = pd.DataFrame({"ocean_proximity": ["INLAND", "NEAR_BAY"]})
>>> pd.get_dummies(df_test)
    ocean_proximity_INLAND  ocean_proximity_NEAR_BAY
0    1    0
1    0    1

Parece legal e simples, então por que não usá-lo em vez do OneHotEncoder? Bem, a vantagem do OneHotEncoder é que ele se lembra de quais categorias foram treinadas. Isso é muito importante porque, uma vez que seu modelo está em produção, ele deve ser alimentado exatamente com os mesmos recursos que durante o treinamento: nem mais, nem menos. Veja o que nosso cat_encoder treinado gera quando o fazemos transformar o mesmo df_test (usando transform(), não fit_transform());

>>> cat_encoder.transform(df_test)
    array([[0., 1., 0., 0., 0.],
    [0., 0., 0., 1., 0.]])

Veja a diferença? get_dummies() viu apenas duas categorias, então gerou duas colunas, enquanto o OneHotEncoder gerou uma coluna por categoria aprendida, na ordem correta. Além disso, se você alimentar get_dummies() com um DataFrame contendo uma categoria desconhecida (por exemplo, "<2H OCEAN"), ele gerará felizmente uma coluna para ela:

>>> df_test_unknown = pd.DataFrame({"ocean_proximity": ["<2H OCEAN", "ISLAND"]})
>>> pd.get_dummies(df_test_unknown)
    ocean_proximity_<2H OCEAN  ocean_proximity_ISLAND
0    1    0
1    0    1

Mas o OneHotEncoder é mais inteligente: ele detectará a categoria desconhecida e levantará uma exceção. Se preferir, você pode definir o hiperparâmetro handle_unknown como "ignore", caso em que ele apenas representará a categoria desconhecida com zeros:

>>> cat_encoder.handle_unknown = "ignore"
>>> cat_encoder.transform(df_test_unknown)
array([[0., 0., 0., 0., 0.],
    [0., 0., 1., 0., 0.]])

**DICA**

Se um atributo categórico tiver um grande número de categorias possíveis (por exemplo, código do país, profissão, espécie), a codificação one-hot resultará em um grande número de recursos de entrada. Isso pode retardar o treinamento e degradar o desempenho. Se isso acontecer, você pode querer substituir a entrada categórica por recursos numéricos úteis relacionados às categorias: por exemplo, você poderia substituir o recurso ocean_proximity pela distância até o oceano (da mesma forma, um código de país poderia ser substituído pela população do país e pelo PIB per capita). Alternativamente, você pode usar um dos codificadores fornecidos pelo pacote category_encoders no GitHub. Ou, ao lidar com redes neurais, você pode substituir cada categoria por um vetor aprendível de baixa dimensão chamado embedding. Este é um exemplo de aprendizado de representação (veja os Capítulos 13 e 17 para mais detalhes).

Quando você ajusta qualquer estimador do Scikit-Learn usando um DataFrame, o estimador armazena os nomes das colunas no atributo feature_names_in_. O Scikit-Learn então garante que qualquer DataFrame alimentado a este estimador depois disso (por exemplo, para transform() ou predict()) tenha os mesmos nomes de colunas. Os transformadores também fornecem um método get_feature_names_out() que você pode usar para construir um DataFrame em torno da saída do transformador:

>>> cat_encoder.feature_names_in_array(['ocean_proximity'], dtype=object)
>>> cat_encoder.get_feature_names_out()
array(['ocean_proximity.<IH OCEAN', 'ocean_proximity_INLAND',
    'ocean_proximity_ISLAND', 'ocean_proximity_NEAR BAY',
    'ocean_proximity_NEAR OCEAN'], dtype=object)
>>> df_output = pd.DataFrame(cat_encoder.transform(df_test_unknown),
    columns=cat_encoder.get_feature_names_out(),
    index=df_test_unknown.index)
...

---

# Escalonamento e Transformação de Recursos

Uma das transformações mais importantes que você precisa aplicar aos seus dados é o *escalonamento de recursos*. Com poucas exceções, os algoritmos de machine learning não funcionam bem quando os atributos numéricos de entrada têm escalas muito diferentes. Este é o caso dos dados de habitação: o número total de quartos varia de cerca de 6 a 39.320, enquanto as rendas médias variam apenas de 0 a 15. Sem nenhum escalonamento, a maioria dos modelos será tendenciosa a ignorar a renda média e se concentrar mais no número de quartos.

Há duas maneiras comuns de fazer com que todos os atributos tenham a mesma escala: *escalonamento min-max* e *padronização*.

---

## AVISO

Como com todos os estimadores, é importante ajustar os escalonadores apenas aos dados de treinamento: nunca use \( \text{fit()} \) ou \( \text{fit_transform()} \) para qualquer coisa que não seja o conjunto de treinamento. Uma vez que você tenha um escalonador treinado, você pode então usá-lo para \( \text{transform()} \) qualquer outro conjunto, incluindo o conjunto de validação, o conjunto de teste e novos dados. Observe que, embora os valores do conjunto de treinamento sempre sejam escalonados para o intervalo especificado, se novos dados contiverem outliers, eles podem acabar escalonados fora do intervalo. Se você quiser evitar isso, basta definir o hiperparâmetro clip como True.

---

O escalonamento min-max (muitas pessoas chamam isso de *normalização*) é o mais simples: para cada atributo, os valores são deslocados e redimensionados para que acabem variando de 0 a 1. Isso é feito subtraindo o valor mínimo e dividindo pela diferença entre o mínimo e o máximo. O Scikit-Learn fornece um transformador chamado MinMaxScaler para isso. Ele tem um hiperparâmetro feature_range que permite alterar o intervalo se, por algum motivo, você não quiser 0–1 (por exemplo, redes neurais funcionam melhor com entradas de média zero, então um intervalo de –1 a 1 é preferível). É bastante fácil de usar:

from sklearn.preprocessing import MinMaxScaler

min_max_scaler = MinMaxScaler(feature_range=(-1, 1))  
housing_num_min_max_scaled = min_max_scaler.fit_transform(housing_num)

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

