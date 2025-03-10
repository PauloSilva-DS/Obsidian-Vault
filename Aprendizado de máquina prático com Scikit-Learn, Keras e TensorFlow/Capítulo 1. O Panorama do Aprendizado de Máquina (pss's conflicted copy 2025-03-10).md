---
tags:
  - estudo
  - python
Completo: false
Atualizado: 2025-03-07  15.52
Criado: 2025-03-07  15.07
---
[[Nova Notas/Aprendizado de máquina]]


---

### **Parte I. Os Fundamentos do Aprendizado de Máquina**

#### **Capítulo 1. O Panorama do Aprendizado de Máquina**

Há não muito tempo, se você pegasse seu telefone e perguntasse o caminho para casa, ele provavelmente o ignoraria — e as pessoas questionariam sua sanidade. Mas o aprendizado de máquina não é mais ficção científica: bilhões de pessoas o usam todos os dias. E a verdade é que ele já existe há décadas em algumas aplicações especializadas, como o reconhecimento óptico de caracteres (OCR). A primeira aplicação de aprendizado de máquina que realmente se tornou popular, melhorando a vida de centenas de milhões de pessoas, conquistou o mundo nos anos 1990: o **filtro de spam**. Não é exatamente um robô autoconsciente, mas tecnicamente se qualifica como aprendizado de máquina: ele realmente aprendeu tão bem que você raramente precisa marcar um e-mail como spam. Ele foi seguido por centenas de aplicações de aprendizado de máquina que agora alimentam silenciosamente centenas de produtos e funcionalidades que você usa regularmente: comandos de voz, tradução automática, busca de imagens, recomendações de produtos e muito mais.

Onde começa e onde termina o aprendizado de máquina? O que exatamente significa uma máquina aprender algo? Se eu baixar uma cópia de todos os artigos da Wikipedia, meu computador realmente aprendeu algo? Ele ficou mais inteligente de repente? Neste capítulo, começarei esclarecendo o que é aprendizado de máquina e por que você pode querer usá-lo.

Então, antes de explorarmos o continente do aprendizado de máquina, daremos uma olhada no mapa e aprenderemos sobre as principais regiões e os marcos mais notáveis: aprendizado **supervisionado** versus **não supervisionado** e suas variantes, aprendizado **online** versus **em lote**, aprendizado **baseado em instância** versus **baseado em modelo**. Depois, veremos o fluxo de trabalho típico de um projeto de aprendizado de máquina, discutiremos os principais desafios que você pode enfrentar e abordaremos como avaliar e ajustar um sistema de aprendizado de máquina.

Este capítulo introduz muitos conceitos fundamentais (e jargões) que todo cientista de dados deve saber de cor. Será uma visão de alto nível (é o único capítulo sem muito código), tudo bastante simples, mas meu objetivo é garantir que tudo esteja cristalino para você antes de continuarmos com o restante do livro. Então, pegue um café e vamos começar!

**DICA**  
Se você já está familiarizado com os conceitos básicos de aprendizado de máquina, pode pular diretamente para o Capítulo 2. Se não tem certeza, tente responder a todas as perguntas listadas no final do capítulo antes de prosseguir.

---

#### **O Que É Aprendizado de Máquina?**

Aprendizado de máquina é a ciência (e arte) de programar computadores para que eles possam aprender com dados.

Aqui está uma definição um pouco mais geral:

> [Aprendizado de máquina é o] campo de estudo que dá aos computadores a capacidade de aprender sem serem explicitamente programados.  
> — Arthur Samuel, 1959

E uma definição mais orientada para a engenharia:

> Um programa de computador é dito aprender com a experiência \( E \) em relação a alguma tarefa \( T \) e alguma medida de desempenho \( P \), se seu desempenho em \( T \), medido por \( P \), melhorar com a experiência \( E \).  
> — Tom Mitchell, 1997

Seu filtro de spam é um programa de aprendizado de máquina que, dados exemplos de e-mails de spam (marcados pelos usuários) e exemplos de e-mails regulares (não spam, também chamados de "ham"), pode aprender a marcar spam. Os exemplos que o sistema usa para aprender são chamados de **conjunto de treinamento**. Cada exemplo de treinamento é chamado de **instância de treinamento** (ou amostra). A parte de um sistema de aprendizado de máquina que aprende e faz previsões é chamada de **modelo**. Redes neurais e florestas aleatórias são exemplos de modelos.

Neste caso, a tarefa \( T \) é marcar spam para novos e-mails, a experiência \( E \) são os dados de treinamento, e a medida de desempenho \( P \) precisa ser definida; por exemplo, você pode usar a proporção de e-mails classificados corretamente. Essa medida de desempenho em particular é chamada de **acurácia**, e é frequentemente usada em tarefas de classificação.

Se você simplesmente baixar uma cópia de todos os artigos da Wikipedia, seu computador terá muito mais dados, mas não ficará subitamente melhor em nenhuma tarefa. Isso não é aprendizado de máquina.

---

#### **Por Que Usar Aprendizado de Máquina?**

Considere como você escreveria um filtro de spam usando técnicas de programação tradicionais (Figura 1-1):

1. Primeiro, você examinaria como o spam geralmente se parece. Você pode notar que algumas palavras ou frases (como "4U", "cartão de crédito", "grátis" e "incrível") tendem a aparecer muito na linha de assunto. Talvez você também notasse alguns outros padrões no nome do remetente, no corpo do e-mail e em outras partes do e-mail.
2. Você escreveria um algoritmo de detecção para cada um dos padrões que notou, e seu programa marcaria e-mails como spam se vários desses padrões fossem detectados.
3. Você testaria seu programa e repetiria os passos 1 e 2 até que ele estivesse bom o suficiente para ser lançado.

**Figura 1-1. A abordagem tradicional**  
Como o problema é difícil, seu programa provavelmente se tornará uma longa lista de regras complexas — bastante difícil de manter.

Em contraste, um filtro de spam baseado em técnicas de aprendizado de máquina aprende automaticamente quais palavras e frases são bons preditores de spam, detectando padrões incomuns de palavras nos exemplos de spam em comparação com os exemplos de ham (Figura 1-2). O programa é muito mais curto, fácil de manter e, muito provavelmente, mais preciso.

**Figura 1-2. A abordagem de aprendizado de máquina**  
E se os spammers perceberem que todos os seus e-mails contendo "4U" são bloqueados? Eles podem começar a escrever "For U" em vez disso. Um filtro de spam usando técnicas de programação tradicionais precisaria ser atualizado para marcar e-mails com "For U". Se os spammers continuarem contornando seu filtro de spam, você precisará continuar escrevendo novas regras para sempre.

Em contraste, um filtro de spam baseado em técnicas de aprendizado de máquina percebe automaticamente que "For U" se tornou incomumente frequente nos spams marcados pelos usuários e começa a marcá-los sem sua intervenção (Figura 1-3).

**Figura 1-3. Adaptando-se automaticamente às mudanças**  
Outra área onde o aprendizado de máquina se destaca é para problemas que são muito complexos para abordagens tradicionais ou para os quais não há algoritmo conhecido. Por exemplo, considere o reconhecimento de fala. Digamos que você queira começar de forma simples e escrever um programa capaz de distinguir as palavras "um" e "dois". Você pode notar que a palavra "dois" começa com um som agudo ("T"), então você poderia codificar um algoritmo que mede a intensidade do som agudo e usá-lo para distinguir "um" e "dois" — mas, obviamente, essa técnica não escalaria para milhares de palavras faladas por milhões de pessoas muito diferentes em ambientes ruidosos e em dezenas de idiomas. A melhor solução (pelo menos hoje) é escrever um algoritmo que aprenda sozinho, dados muitos exemplos de gravações para cada palavra.

Finalmente, o aprendizado de máquina pode ajudar os humanos a aprender (Figura 1-4). Modelos de aprendizado de máquina podem ser inspecionados para ver o que aprenderam (embora, para alguns modelos, isso possa ser complicado). Por exemplo, uma vez que um filtro de spam foi treinado com spam suficiente, ele pode ser facilmente inspecionado para revelar a lista de palavras e combinações de palavras que ele acredita serem os melhores preditores de spam. Às vezes, isso revelará correlações inesperadas ou novas tendências, levando a uma melhor compreensão do problema. Cavar grandes quantidades de dados para descobrir padrões ocultos é chamado de **mineração de dados**, e o aprendizado de máquina se destaca nisso.

**Figura 1-4. O aprendizado de máquina pode ajudar os humanos a aprender**  
Para resumir, o aprendizado de máquina é ótimo para:
- **Problemas para os quais as soluções existentes exigem muito ajuste fino ou longas listas de regras**: um modelo de aprendizado de máquina pode simplificar o código e ter um desempenho melhor que a abordagem tradicional.
- **Problemas complexos para os quais o uso de uma abordagem tradicional não oferece uma boa solução**: as melhores técnicas de aprendizado de máquina podem talvez encontrar uma solução.
- **Ambientes flutuantes**: um sistema de aprendizado de máquina pode ser facilmente retreinado com novos dados, mantendo-se sempre atualizado.
- **Obter insights sobre problemas complexos e grandes quantidades de dados**.

---

#### **Exemplos de Aplicações**

Vejamos alguns exemplos concretos de tarefas de aprendizado de máquina, juntamente com as técnicas que podem resolvê-las:

- **Analisar imagens de produtos em uma linha de produção para classificá-los automaticamente**:  
  Isso é **classificação de imagens**, tipicamente realizada usando redes neurais convolucionais (CNNs; veja o Capítulo 14) ou, às vezes, transformadores (veja o Capítulo 16).

- **Detectar tumores em exames cerebrais**:  
  Isso é **segmentação semântica de imagens**, onde cada pixel na imagem é classificado (pois queremos determinar a localização exata e a forma dos tumores), tipicamente usando CNNs ou transformadores.

- **Classificar automaticamente artigos de notícias**:  
  Isso é **processamento de linguagem natural (NLP)** e, mais especificamente, **classificação de texto**, que pode ser abordada usando redes neurais recorrentes (RNNs) e CNNs, mas os transformadores funcionam ainda melhor (veja o Capítulo 16).

- **Sinalizar automaticamente comentários ofensivos em fóruns de discussão**:  
  Isso também é **classificação de texto**, usando as mesmas ferramentas de NLP.

- **Resumir automaticamente documentos longos**:  
  Isso é uma ramificação do NLP chamada **sumarização de texto**, novamente usando as mesmas ferramentas.

- **Criar um chatbot ou um assistente pessoal**:  
  Isso envolve muitos componentes de NLP, incluindo módulos de **compreensão de linguagem natural (NLU)** e de **resposta a perguntas**.

- **Prever a receita da sua empresa no próximo ano, com base em muitas métricas de desempenho**:  
  Isso é uma tarefa de **regressão** (ou seja, prever valores) que pode ser abordada usando qualquer modelo de regressão, como um modelo de regressão linear ou polinomial (veja o Capítulo 4), uma máquina de vetores de suporte para regressão (veja o Capítulo 5), uma floresta aleatória para regressão (veja o Capítulo 7) ou uma rede neural artificial (veja o Capítulo 10). Se você quiser levar em consideração sequências de métricas de desempenho passadas, pode usar RNNs, CNNs ou transformadores (veja os Capítulos 15 e 16).

- **Fazer seu aplicativo reagir a comandos de voz**:  
  Isso é **reconhecimento de fala**, que requer o processamento de amostras de áudio: como são sequências longas e complexas, são tipicamente processadas usando RNNs, CNNs ou transformadores (veja os Capítulos 15 e 16).

- **Detectar fraudes em cartões de crédito**:  
  Isso é **detecção de anomalias**, que pode ser abordada usando florestas de isolamento, modelos de mistura gaussiana (veja o Capítulo 9) ou autoencoders (veja o Capítulo 17).

- **Segmentar clientes com base em suas compras para que você possa projetar uma estratégia de marketing diferente para cada segmento**:  
  Isso é **clustering**, que pode ser alcançado usando \( k \)-means, DBSCAN e outros (veja o Capítulo 9).

- **Representar um conjunto de dados complexo e de alta dimensionalidade em um diagrama claro e perspicaz**:  
  Isso é **visualização de dados**, muitas vezes envolvendo técnicas de redução de dimensionalidade (veja o Capítulo 8).

- **Recomendar um produto que um cliente pode estar interessado, com base em compras anteriores**:  
  Isso é um **sistema de recomendação**. Uma abordagem é alimentar as compras anteriores (e outras informações sobre o cliente) em uma rede neural artificial (veja o Capítulo 10) e fazê-la prever a próxima compra mais provável. Essa rede neural seria tipicamente treinada em sequências passadas de compras de todos os clientes.

- **Construir um bot inteligente para um jogo**:  
  Isso é frequentemente abordado usando **aprendizado por reforço (RL; veja o Capítulo 18)**, que é um ramo do aprendizado de máquina que treina agentes (como bots) para escolher as ações que maximizarão suas recompensas ao longo do tempo (por exemplo, um bot pode receber uma recompensa toda vez que o jogador perder pontos de vida), dentro de um determinado ambiente (como o jogo). O famoso programa AlphaGo, que derrotou o campeão mundial no jogo Go, foi construído usando RL.

Essa lista poderia continuar, mas espero que ela dê uma ideia da incrível amplitude e complexidade das tarefas que o aprendizado de máquina pode abordar e dos tipos de técnicas que você usaria para cada tarefa.

---

#### **Tipos de Sistemas de Aprendizado de Máquina**

Existem tantos tipos diferentes de sistemas de aprendizado de máquina que é útil classificá-los em categorias amplas, com base nos seguintes critérios:

1. **Como eles são supervisionados durante o treinamento**: supervisionado, não supervisionado, semi-supervisionado, auto-supervisionado e outros.
2. **Se eles podem aprender incrementalmente em tempo real**: online versus em lote.
3. **Se eles funcionam simplesmente comparando novos pontos de dados a pontos de dados conhecidos ou, em vez disso, detectando padrões nos dados de treinamento e construindo um modelo preditivo, como cientistas fazem**: baseado em instância versus baseado em modelo.

Esses critérios não são exclusivos; você pode combiná-los da maneira que quiser. Por exemplo, um filtro de spam state-of-the-art pode aprender em tempo real usando um modelo de rede neural profunda treinado com exemplos de spam e ham fornecidos por humanos; isso o torna um sistema de aprendizado online, baseado em modelo e supervisionado.

Vamos examinar cada um desses critérios mais de perto.

---

#### **Supervisão de Treinamento**

Os sistemas de aprendizado de máquina podem ser classificados de acordo com a quantidade e o tipo de supervisão que recebem durante o treinamento. Existem muitas categorias, mas discutiremos as principais: aprendizado supervisionado, não supervisionado, auto-supervisionado, semi-supervisionado e aprendizado por reforço.

---

#### **Aprendizado Supervisionado**

No aprendizado supervisionado, o conjunto de treinamento que você alimenta ao algoritmo inclui as soluções desejadas, chamadas de **rótulos** (Figura 1-5).

**Figura 1-5. Um conjunto de treinamento rotulado para classificação de spam (um exemplo de aprendizado supervisionado)**

Uma tarefa típica de aprendizado supervisionado é a **classificação**. O filtro de spam é um bom exemplo disso: ele é treinado com muitos exemplos de e-mails junto com sua classe (spam ou ham), e ele deve aprender como classificar novos e-mails.

Outra tarefa típica é prever um valor numérico, como o preço de um carro, dado um conjunto de características (quilometragem, idade, marca, etc.). Esse tipo de tarefa é chamado de **regressão** (Figura 1-6). Para treinar o sistema, você precisa fornecer muitos exemplos de carros, incluindo suas características e seus valores alvo (ou seja, seus preços).

**Figura 1-6. Um problema de regressão: prever um valor, dado um recurso de entrada (geralmente há vários recursos de entrada e, às vezes, vários valores de saída)**

**NOTA**  
As palavras **target** e **label** são geralmente tratadas como sinônimos em aprendizado supervisionado, mas **target** é mais comum em tarefas de regressão e **label** é mais comum em tarefas de classificação. Além disso, **features** às vezes são chamadas de **predictors** ou **attributes**. Esses termos podem se referir a amostras individuais (por exemplo, "a quilometragem deste carro é 15.000") ou a todas as amostras (por exemplo, "a quilometragem está fortemente correlacionada com o preço").

---

#### **Aprendizado Não Supervisionado**

No aprendizado não supervisionado, como você pode imaginar, os dados de treinamento não são rotulados (Figura 1-7). O sistema tenta aprender sem um professor.

**Figura 1-7. Um conjunto de treinamento não rotulado para aprendizado não supervisionado**

Por exemplo, digamos que você tenha muitos dados sobre os visitantes do seu blog. Você pode querer executar um algoritmo de **clustering** para tentar detectar grupos de visitantes semelhantes (Figura 1-8). Em nenhum momento você diz ao algoritmo a qual grupo um visitante pertence: ele encontra essas conexões sem sua ajuda. Por exemplo, ele pode notar que 40% dos seus visitantes são adolescentes que adoram quadrinhos e geralmente leem seu blog após a escola, enquanto 20% são adultos que gostam de ficção científica e visitam durante os fins de semana. Se você usar um algoritmo de clustering hierárquico, ele também pode subdividir cada grupo em grupos menores. Isso pode ajudá-lo a direcionar suas postagens para cada grupo.

**Figura 1-8. Clustering**

Algoritmos de **visualização** também são bons exemplos de aprendizado não supervisionado: você os alimenta com muitos dados complexos e não rotulados, e eles produzem uma representação 2D ou 3D dos seus dados que pode ser facilmente plotada (Figura 1-9). Esses algoritmos tentam preservar o máximo de estrutura possível (por exemplo, tentando evitar que clusters separados no espaço de entrada se sobreponham na visualização) para que você possa entender como os dados estão organizados e talvez identificar padrões inesperados.

**Figura 1-9. Exemplo de uma visualização t-SNE destacando clusters semânticos**

**DICA**  
É frequentemente uma boa ideia tentar reduzir o número de dimensões nos seus dados de treinamento usando um algoritmo de redução de dimensionalidade antes de alimentá-los a outro algoritmo de aprendizado de máquina (como um algoritmo de aprendizado supervisionado). Ele rodará muito mais rápido, os dados ocuparão menos espaço em disco e na memória, e em alguns casos também pode ter um desempenho melhor.

Outra tarefa importante de aprendizado não supervisionado é a **detecção de anomalias** — por exemplo, detectar transações incomuns de cartão de crédito para prevenir fraudes, capturar defeitos de fabricação ou remover automaticamente outliers de um conjunto de dados antes de alimentá-lo a outro algoritmo de aprendizado. O sistema é mostrado principalmente instâncias normais durante o treinamento, então ele aprende a reconhecê-las; depois, quando vê uma nova instância, ele pode dizer se ela parece normal ou se é provavelmente uma anomalia (veja a Figura 1-10). Uma tarefa muito semelhante é a **detecção de novidades**: ela visa detectar novas instâncias que parecem diferentes de todas as instâncias no conjunto de treinamento. Isso requer ter um conjunto de treinamento muito "limpo", sem nenhuma instância que você gostaria que o algoritmo detectasse. Por exemplo, se você tiver milhares de fotos de cachorros, e 1% dessas fotos representam Chihuahuas, então um algoritmo de detecção de novidades não deve tratar novas fotos de Chihuahuas como novidades. Por outro lado, algoritmos de detecção de anomalias podem considerar esses cachorros tão raros e diferentes de outros cachorros que provavelmente os classificariam como anomalias (sem ofensa aos Chihuahuas).

**Figura 1-10. Detecção de anomalias**

Finalmente, outra tarefa comum de aprendizado não supervisionado é a **aprendizagem de regras de associação**, na qual o objetivo é cavar grandes quantidades de dados e descobrir relações interessantes entre atributos. Por exemplo, suponha que você tenha um supermercado. Executar uma regra de associação nos seus registros de vendas pode revelar que pessoas que compram molho barbecue e batatas fritas também tendem a comprar bife. Assim, você pode querer colocar esses itens próximos uns dos outros.

---

#### **Aprendizado Semi-Supervisionado**

Como rotular dados geralmente consome tempo e é caro, muitas vezes você terá muitas instâncias não rotuladas e poucas instâncias rotuladas. Alguns algoritmos podem lidar com dados parcialmente rotulados. Isso é chamado de **aprendizado semi-supervisionado** (Figura 1-11).

**Figura 1-11. Aprendizado semi-supervisionado com duas classes (triângulos e quadrados): os exemplos não rotulados (círculos) ajudam a classificar uma nova instância (a cruz) na classe dos triângulos em vez da classe dos quadrados, mesmo que ela esteja mais próxima dos quadrados rotulados**

Alguns serviços de hospedagem de fotos, como o Google Fotos, são bons exemplos disso. Depois que você faz o upload de todas as suas fotos de família para o serviço, ele reconhece automaticamente que a mesma pessoa A aparece nas fotos 1, 5 e 11, enquanto outra pessoa B aparece nas fotos 2, 5 e 7. Essa é a parte não supervisionada do algoritmo (clustering). Agora, tudo o que o sistema precisa é que você diga quem são essas pessoas. Basta adicionar um rótulo por pessoa\(^3\), e ele será capaz de nomear todos em todas as fotos, o que é útil para pesquisar fotos.

A maioria dos algoritmos de aprendizado semi-supervisionado são combinações de algoritmos não supervisionados e supervisionados. Por exemplo, um algoritmo de clustering pode ser usado para agrupar instâncias semelhantes, e então cada instância não rotulada pode ser rotulada com o rótulo mais comum em seu cluster. Depois que todo o conjunto de dados é rotulado, é possível usar qualquer algoritmo de aprendizado supervisionado.

---

#### **Aprendizado Auto-Supervisionado**

Outra abordagem para o aprendizado de máquina envolve gerar um conjunto de dados totalmente rotulado a partir de um conjunto de dados não rotulado. Novamente, uma vez que todo o conjunto de dados é rotulado, qualquer algoritmo de aprendizado supervisionado pode ser usado. Essa abordagem é chamada de **aprendizado auto-supervisionado**.

Por exemplo, se você tiver um grande conjunto de dados de imagens não rotuladas, pode mascarar aleatoriamente uma pequena parte de cada imagem e então treinar um modelo para recuperar a imagem original (Figura 1-12). Durante o treinamento, as imagens mascaradas são usadas como entradas para o modelo, e as imagens originais são usadas como rótulos.

**Figura 1-12. Exemplo de aprendizado auto-supervisionado: entrada (esquerda) e alvo (direita)**

O modelo resultante pode ser bastante útil por si só — por exemplo, para reparar imagens danificadas ou para apagar objetos indesejados de fotos. Mas, na maioria das vezes, um modelo treinado com aprendizado auto-supervisionado não é o objetivo final. Você geralmente desejará ajustar e refinar o modelo para uma tarefa ligeiramente diferente — uma que você realmente se importa.

Por exemplo, suponha que o que você realmente deseja é ter um modelo de classificação de animais de estimação: dada uma foto de qualquer animal de estimação, ele dirá a qual espécie ele pertence. Se você tiver um grande conjunto de dados de fotos não rotuladas de animais de estimação, pode começar treinando um modelo de reparação de imagens usando aprendizado auto-supervisionado. Uma vez que ele estiver funcionando bem, ele deve ser capaz de distinguir diferentes espécies de animais de estimação: quando ele repara uma imagem de um gato cujo rosto está mascarado, ele deve saber não adicionar o rosto de um cachorro. Supondo que a arquitetura do seu modelo permita isso (e a maioria das arquiteturas de redes neurais permite), é possível ajustar o modelo para que ele preveja espécies de animais de estimação em vez de reparar imagens. O passo final consiste em ajustar o modelo em um conjunto de dados rotulado: o modelo já sabe como são gatos, cachorros e outras espécies de animais de estimação, então este passo é necessário apenas para que o modelo aprenda o mapeamento entre as espécies que ele já conhece e os rótulos que esperamos dele.

**NOTA**  
Transferir conhecimento de uma tarefa para outra é chamado de **aprendizado por transferência (transfer learning)**, e é uma das técnicas mais importantes no aprendizado de máquina hoje, especialmente ao usar redes neurais profundas (ou seja, redes neurais compostas por muitas camadas de neurônios). Discutiremos isso em detalhes na Parte II.

Algumas pessoas consideram o aprendizado auto-supervisionado como parte do aprendizado não supervisionado, já que ele lida com conjuntos de dados totalmente não rotulados. Mas o aprendizado auto-supervisionado usa rótulos (gerados) durante o treinamento, então, nesse aspecto, ele está mais próximo do aprendizado supervisionado. E o termo "aprendizado não supervisionado" é geralmente usado ao lidar com tarefas como clustering, redução de dimensionalidade ou detecção de anomalias, enquanto o aprendizado auto-supervisionado se concentra nas mesmas tarefas que o aprendizado supervisionado: principalmente classificação e regressão. Em resumo, é melhor tratar o aprendizado auto-supervisionado como sua própria categoria.

---

#### **Aprendizado por Reforço**

O **aprendizado por reforço** é uma abordagem muito diferente. O sistema de aprendizado, chamado de **agente** nesse contexto, pode observar o ambiente, selecionar e executar ações, e receber recompensas em troca (ou penalidades na forma de recompensas negativas, como mostrado na Figura 1-13). Ele deve então aprender por si mesmo qual é a melhor estratégia, chamada de **política**, para obter a maior recompensa ao longo do tempo. Uma política define qual ação o agente deve escolher quando está em uma determinada situação.

**Figura 1-13. Aprendizado por reforço**

Por exemplo, muitos robôs implementam algoritmos de aprendizado por reforço para aprender a andar. O programa AlphaGo da DeepMind também é um bom exemplo de aprendizado por reforço: ele ganhou as manchetes em maio de 2017 quando derrotou Ke Jie, o jogador número um do mundo na época, no jogo Go. Ele aprendeu sua política vencedora analisando milhões de jogos e, em seguida, jogando muitos jogos contra si mesmo. Observe que o aprendizado foi desativado durante os jogos contra o campeão; o AlphaGo estava apenas aplicando a política que havia aprendido. Como você verá na próxima seção, isso é chamado de **aprendizado offline**.

---

#### **Aprendizado em Lote versus Online**

Outro critério usado para classificar sistemas de aprendizado de máquina é se o sistema pode aprender incrementalmente a partir de um fluxo de dados recebidos.

---

#### **Aprendizado em Lote**

No **aprendizado em lote**, o sistema é incapaz de aprender incrementalmente: ele deve ser treinado usando todos os dados disponíveis. Isso geralmente levará muito tempo e recursos computacionais, então é tipicamente feito offline. Primeiro, o sistema é treinado e, em seguida, é lançado em produção e roda sem aprender mais; ele apenas aplica o que aprendeu. Isso é chamado de **aprendizado offline**.

Infelizmente, o desempenho de um modelo tende a decair lentamente ao longo do tempo, simplesmente porque o mundo continua a evoluir enquanto o modelo permanece inalterado. Esse fenômeno é frequentemente chamado de **decaimento do modelo** ou **deriva de dados**. A solução é retreinar regularmente o modelo com dados atualizados. Com que frequência você precisa fazer isso depende do caso de uso: se o modelo classifica fotos de gatos e cachorros, seu desempenho decairá muito lentamente, mas se o modelo lida com sistemas que evoluem rapidamente, por exemplo, fazendo previsões no mercado financeiro, é provável que ele decaia rapidamente.

**AVISO**  
Mesmo um modelo treinado para classificar fotos de gatos e cachorros pode precisar ser retreinado regularmente, não porque gatos e cachorros vão sofrer mutações da noite para o dia, mas porque as câmeras continuam mudando, juntamente com formatos de imagem, nitidez, brilho e proporções de tamanho. Além disso, as pessoas podem amar raças diferentes no próximo ano, ou podem decidir vestir seus animais de estimação com pequenos chapéus — quem sabe?

Se você quiser que um sistema de aprendizado em lote saiba sobre novos dados (como um novo tipo de spam), precisará treinar uma nova versão do sistema do zero no conjunto completo de dados (não apenas os novos dados, mas também os dados antigos), e então substituir o modelo antigo pelo novo. Felizmente, todo o processo de treinamento, avaliação e lançamento de um sistema de aprendizado de máquina pode ser automatizado com relativa facilidade (como vimos na Figura 1-3), então mesmo um sistema de aprendizado em lote pode se adaptar à mudança. Basta atualizar os dados e treinar uma nova versão do sistema do zero quantas vezes for necessário.

---

#### **Aprendizado Online**

No **aprendizado online**, você treina o sistema incrementalmente, alimentando-o com instâncias de dados sequencialmente, individualmente ou em pequenos grupos chamados **mini-lotes**. Cada passo de aprendizado é rápido e barato, então o sistema pode aprender sobre novos dados em tempo real, à medida que eles chegam (veja a Figura 1-14).

**Figura 1-14. No aprendizado online, um modelo é treinado e lançado em produção, e então continua aprendendo à medida que novos dados chegam**

O aprendizado online é útil para sistemas que precisam se adaptar a mudanças extremamente rápidas (por exemplo, para detectar novos padrões no mercado de ações). Também é uma boa opção se você tiver recursos computacionais limitados; por exemplo, se o modelo for treinado em um dispositivo móvel.

Além disso, algoritmos de aprendizado online podem ser usados para treinar modelos em grandes conjuntos de dados que não cabem na memória principal de uma máquina (isso é chamado de **aprendizado fora do núcleo**). O algoritmo carrega parte dos dados, executa um passo de treinamento nesses dados e repete o processo até que tenha processado todos os dados (veja a Figura 1-15).

**Figura 1-15. Usando aprendizado online para lidar com grandes conjuntos de dados**

Um parâmetro importante dos sistemas de aprendizado online é a velocidade com que eles devem se adaptar a mudanças nos dados: isso é chamado de **taxa de aprendizado**. Se você definir uma taxa de aprendizado alta, seu sistema se adaptará rapidamente a novos dados, mas também tenderá a esquecer rapidamente os dados antigos (e você não quer que um filtro de spam marque apenas os tipos mais recentes de spam que ele viu). Por outro lado, se você definir uma taxa de aprendizado baixa, o sistema terá mais inércia; ou seja, ele aprenderá mais lentamente, mas também será menos sensível a ruídos nos novos dados ou a sequências de pontos de dados não representativos (outliers).

**AVISO**  
O aprendizado fora do núcleo geralmente é feito offline (ou seja, não no sistema ao vivo), então o termo "aprendizado online" pode ser confuso. Pense nele como **aprendizado incremental**.

Um grande desafio com o aprendizado online é que, se dados ruins forem alimentados ao sistema, o desempenho do sistema pode cair, possivelmente rapidamente (dependendo da qualidade dos dados e da taxa de aprendizado). Se for um sistema ao vivo, seus clientes notarão. Por exemplo, dados ruins podem vir de um bug (por exemplo, um sensor com defeito em um robô) ou de alguém tentando manipular o sistema (por exemplo, spammando um mecanismo de busca para tentar se classificar bem nos resultados de pesquisa). Para reduzir esse risco, você precisa monitorar seu sistema de perto e desligar o aprendizado prontamente (e possivelmente reverter para um estado anterior que funcionava) se detectar uma queda no desempenho. Você também pode querer monitorar os dados de entrada e reagir a dados anormais; por exemplo, usando um algoritmo de detecção de anomalias (veja o Capítulo 9).

---

#### **Aprendizado Baseado em Instância versus Baseado em Modelo**

Outra maneira de categorizar sistemas de aprendizado de máquina é pela forma como eles **generalizam**. A maioria das tarefas de aprendizado de máquina envolve fazer previsões. Isso significa que, dado um número de exemplos de treinamento, o sistema precisa ser capaz de fazer boas previsões para (generalizar para) exemplos que nunca viu antes. Ter uma boa medida de desempenho nos dados de treinamento é bom, mas insuficiente; o verdadeiro objetivo é ter um bom desempenho em novas instâncias.

Existem duas abordagens principais para a generalização: **aprendizado baseado em instância** e **aprendizado baseado em modelo**.

---

#### **Aprendizado Baseado em Instância**

Possivelmente a forma mais trivial de aprendizado é simplesmente aprender de cor. Se você fosse criar um filtro de spam dessa maneira, ele apenas marcaria todos os e-mails que são idênticos a e-mails que já foram marcados pelos usuários — não a pior solução, mas certamente não a melhor.

Em vez de apenas marcar e-mails idênticos a e-mails de spam conhecidos, seu filtro de spam poderia ser programado para também marcar e-mails que são muito semelhantes a e-mails de spam conhecidos. Isso requer uma medida de similaridade entre dois e-mails. Uma medida de similaridade (muito básica) entre dois e-mails poderia ser contar o número de palavras que eles têm em comum. O sistema marcaria um e-mail como spam se ele tivesse muitas palavras em comum com um e-mail de spam conhecido.

Isso é chamado de **aprendizado baseado em instância**: o sistema aprende os exemplos de cor e, em seguida, generaliza para novos casos usando uma medida de similaridade para compará-los aos exemplos aprendidos (ou a um subconjunto deles). Por exemplo, na Figura 1-16, a nova instância seria classificada como um triângulo porque a maioria das instâncias mais semelhantes pertence a essa classe.

**Figura 1-16. Aprendizado baseado em instância**

---

#### **Aprendizado Baseado em Modelo e um Fluxo de Trabalho Típico de Aprendizado de Máquina**

Outra maneira de generalizar a partir de um conjunto de exemplos é construir um **modelo** desses exemplos e, em seguida, usar esse modelo para fazer previsões. Isso é chamado de **aprendizado baseado em modelo** (Figura 1-17).

**Figura 1-17. Aprendizado baseado em modelo**

Por exemplo, suponha que você queira saber se o dinheiro torna as pessoas mais felizes, então você baixa os dados do Índice de Vida Melhor da OCDE e as estatísticas do Banco Mundial sobre o produto interno bruto (PIB) per capita. Em seguida, você une as tabelas e as ordena por PIB per capita. A Tabela 1-1 mostra um trecho do que você obtém.

**Tabela 1-1. O dinheiro torna as pessoas mais felizes?**

| País         | PIB per capita (USD) | Satisfação com a vida |
|--------------|----------------------|-----------------------|
| Turquia      | 28.384               | 5,5                   |
| Hungria      | 31.008               | 5,6                   |
| França       | 42.026               | 6,5                   |
| Estados Unidos | 60.236               | 6,9                   |
| Nova Zelândia | 42.404               | 7,3                   |
| Austrália    | 48.698               | 7,3                   |
| Dinamarca    | 55.938               | 7,6                   |

Vamos plotar os dados para esses países (Figura 1-18).

**Figura 1-18. Você vê uma tendência aqui?**

Parece haver uma tendência aqui! Embora os dados sejam **ruidosos** (ou seja, parcialmente aleatórios), parece que a satisfação com a vida aumenta mais ou menos linearmente à medida que o PIB per capita do país aumenta. Então você decide modelar a satisfação com a vida como uma função linear do PIB per capita. Esse passo é chamado de **seleção de modelo**: você selecionou um **modelo linear** de satisfação com a vida com apenas um atributo, o PIB per capita (Equação 1-1).

**Equação 1-1. Um modelo linear simples**

\[ \text{satisfação\_com\_a\_vida} = \theta_0 + \theta_1 \times \text{PIB\_per\_capita} \]

Esse modelo tem dois **parâmetros do modelo**, \(\theta_0\) e \(\theta_1\). Ao ajustar esses parâmetros, você pode fazer seu modelo representar qualquer função linear, como mostrado na Figura 1-19.

**Figura 1-19. Alguns possíveis modelos lineares**

Antes de usar seu modelo, você precisa definir os valores dos parâmetros \(\theta_0\) e \(\theta_1\). Como você pode saber quais valores farão seu modelo ter o melhor desempenho? Para responder a essa pergunta, você precisa especificar uma medida de desempenho. Você pode definir uma **função de utilidade** (ou **função de aptidão**) que mede o quão **bom** seu modelo é, ou pode definir uma **função de custo** que mede o quão **ruim** ele é. Para problemas de regressão linear, as pessoas geralmente usam uma função de custo que mede a distância entre as previsões do modelo linear e os exemplos de treinamento; o objetivo é minimizar essa distância.

É aí que entra o algoritmo de regressão linear: você o alimenta com seus exemplos de treinamento, e ele encontra os parâmetros que fazem o modelo linear se ajustar melhor aos seus dados. Isso é chamado de **treinamento** do modelo. No nosso caso, o algoritmo descobre que os valores ótimos dos parâmetros são \(\theta_0 = 3,75\) e \(\theta_1 = 6,78 \times 10^{-5}\).

**AVISO**  
Confusamente, a palavra "modelo" pode se referir a um tipo de modelo (por exemplo, regressão linear), a uma arquitetura de modelo totalmente especificada (por exemplo, regressão linear com uma entrada e uma saída) ou ao modelo final treinado e pronto para ser usado para previsões (por exemplo, regressão linear com uma entrada e uma saída, usando \(\theta_0 = 3,75\) e \(\theta_1 = 6,78 \times 10^{-5}\)). A seleção de modelo consiste em escolher o tipo de modelo e especificar completamente sua arquitetura. Treinar um modelo significa executar um algoritmo para encontrar os parâmetros do modelo que o farão se ajustar melhor aos dados de treinamento e, esperançosamente, fazer boas previsões em novos dados.

Agora o modelo se ajusta aos dados de treinamento o mais próximo possível (para um modelo linear), como você pode ver na Figura 1-20.

**Figura 1-20. O modelo linear que melhor se ajusta aos dados de treinamento**

Você está finalmente pronto para executar o modelo e fazer previsões. Por exemplo, digamos que você queira saber o quão felizes são os cipriotas, e os dados da OCDE não têm a resposta. Felizmente, você pode usar seu modelo para fazer uma boa previsão: você consulta o PIB per capita de Chipre, encontra $37.655 e, em seguida, aplica seu modelo e descobre que a satisfação com a vida provavelmente está em torno de \(3,75 + 37.655 \times 6,78 \times 10^{-5} = 6,30\).

---

#### **Desafios Principais do Aprendizado de Máquina**

Em resumo, como sua principal tarefa é selecionar um modelo e treiná-lo com alguns dados, as duas coisas que podem dar errado são "modelo ruim" e "dados ruins". Vamos começar com exemplos de dados ruins.

---

#### **Quantidade Insuficiente de Dados de Treinamento**

Para uma criança aprender o que é uma maçã, basta apontar para uma maçã e dizer "maçã" (possivelmente repetindo esse procedimento algumas vezes). Agora a criança é capaz de reconhecer maçãs em todas as cores e formas. Gênio.

O aprendizado de máquina ainda não chegou lá; é preciso muita data para a maioria dos algoritmos de aprendizado de máquina funcionarem corretamente. Mesmo para problemas muito simples, você geralmente precisa de milhares de exemplos, e para problemas complexos, como reconhecimento de imagem ou fala, você pode precisar de milhões de exemplos (a menos que você possa reutilizar partes de um modelo existente).

---

#### **Dados de Treinamento Não Representativos**

Para generalizar bem, é crucial que seus dados de treinamento sejam representativos dos novos casos para os quais você deseja generalizar. Isso é verdade, quer você use aprendizado baseado em instância ou baseado em modelo.

Por exemplo, o conjunto de países que você usou anteriormente para treinar o modelo linear não era perfeitamente representativo; ele não continha nenhum país com um PIB per capita inferior a $23.500 ou superior a $62.500. A Figura 1-22 mostra como os dados ficam quando você adiciona esses países.

Se você treinar um modelo linear nesses dados, obterá a linha sólida, enquanto o modelo antigo é representado pela linha pontilhada. Como você pode ver, não apenas a adição de alguns países ausentes altera significativamente o modelo, mas também deixa claro que um modelo linear tão simples provavelmente nunca funcionará bem. Parece que países muito ricos não são mais felizes do que países moderadamente ricos (na verdade, eles parecem um pouco menos felizes!), e, inversamente, alguns países pobres parecem mais felizes do que muitos países ricos.

Ao usar um conjunto de treinamento não representativo, você treinou um modelo que provavelmente não fará previsões precisas, especialmente para países muito pobres e muito ricos.

**Figura 1-22. Uma amostra de treinamento mais representativa**

É crucial usar um conjunto de treinamento que seja representativo dos casos para os quais você deseja generalizar. Isso é frequentemente mais difícil do que parece: se a amostra for muito pequena, você terá **ruído de amostragem** (ou seja, dados não representativos como resultado do acaso), mas mesmo amostras muito grandes podem ser não representativas se o método de amostragem for falho. Isso é chamado de **viés de amostragem**.

---

#### **Exemplos de Viés de Amostragem**

Talvez o exemplo mais famoso de viés de amostragem tenha ocorrido durante a eleição presidencial dos EUA em 1936, que opôs Landon a Roosevelt: o *Literary Digest* realizou uma pesquisa muito grande, enviando correspondência para cerca de 10 milhões de pessoas. Ele recebeu 2,4 milhões de respostas e previu com alta confiança que Landon receberia 57% dos votos. Em vez disso, Roosevelt venceu com 62% dos votos. O erro estava no método de amostragem do *Literary Digest*:

1. Primeiro, para obter os endereços para enviar as pesquisas, o *Literary Digest* usou listas telefônicas, listas de assinantes de revistas, listas de membros de clubes e afins. Todas essas listas tendiam a favorecer pessoas mais ricas, que eram mais propensas a votar no Partido Republicano (daí Landon).
2. Segundo, menos de 25% das pessoas que foram pesquisadas responderam. Isso introduziu um viés de amostragem, potencialmente excluindo pessoas que não se importavam muito com política, pessoas que não gostavam do *Literary Digest* e outros grupos importantes. Isso é um tipo especial de viés de amostragem chamado **viés de não resposta**.

Aqui está outro exemplo: digamos que você queira construir um sistema para reconhecer vídeos de música funk. Uma maneira de construir seu conjunto de treinamento é pesquisar por "música funk" no YouTube e usar os vídeos resultantes. Mas isso assume que o mecanismo de busca do YouTube retorna um conjunto de vídeos que são representativos de todos os vídeos de música funk no YouTube. Na realidade, os resultados da pesquisa provavelmente serão tendenciosos em favor de artistas populares (e se você mora no Brasil, receberá muitos vídeos de "funk carioca", que não têm nada a ver com James Brown). Por outro lado, como mais você pode obter um grande conjunto de treinamento?

---

#### **Dados de Baixa Qualidade**

Obviamente, se seus dados de treinamento estiverem cheios de erros, outliers e ruído (por exemplo, devido a medições de baixa qualidade), será mais difícil para o sistema detectar os padrões subjacentes, então é menos provável que seu sistema tenha um bom desempenho. Muitas vezes vale a pena gastar tempo limpando seus dados de treinamento. A verdade é que a maioria dos cientistas de dados passa uma parte significativa de seu tempo fazendo exatamente isso. Aqui estão alguns exemplos de quando você pode querer limpar os dados de treinamento:

- Se algumas instâncias forem claramente outliers, pode ajudar simplesmente descartá-las ou tentar corrigir os erros manualmente.
- Se algumas instâncias estiverem faltando alguns recursos (por exemplo, 5% dos seus clientes não especificaram sua idade), você deve decidir se deseja ignorar esse atributo completamente, ignorar essas instâncias, preencher os valores ausentes (por exemplo, com a idade mediana) ou treinar um modelo com o recurso e outro sem ele.

---

#### **Recursos Irrelevantes**

Como diz o ditado: lixo entra, lixo sai. Seu sistema só será capaz de aprender se os dados de treinamento contiverem recursos relevantes suficientes e não muitos irrelevantes. Uma parte crítica do sucesso de um projeto de aprendizado de máquina é criar um bom conjunto de recursos para treinar. Esse processo, chamado de **engenharia de recursos**, envolve as seguintes etapas:

1. **Seleção de recursos**: selecionar os recursos mais úteis para treinar entre os recursos existentes.
2. **Extração de recursos**: combinar recursos existentes para produzir um mais útil — como vimos anteriormente, algoritmos de redução de dimensionalidade podem ajudar.
3. **Criação de novos recursos**: coletar novos dados.

Agora que vimos muitos exemplos de dados ruins, vamos dar uma olhada em alguns exemplos de algoritmos ruins.

---

#### **Sobreajuste aos Dados de Treinamento**

Digamos que você está visitando um país estrangeiro e o motorista de táxi tenta te enganar. Você pode ser tentado a dizer que **todos** os motoristas de táxi daquele país são ladrões. Generalizar demais é algo que nós, humanos, fazemos com frequência, e, infelizmente, as máquinas podem cair na mesma armadilha se não formos cuidadosos. No aprendizado de máquina, isso é chamado de **sobreajuste**: significa que o modelo tem um bom desempenho nos dados de treinamento, mas não generaliza bem.

A Figura 1-23 mostra um exemplo de um modelo polinomial de alto grau de satisfação com a vida que se ajusta excessivamente aos dados de treinamento. Embora ele tenha um desempenho muito melhor nos dados de treinamento do que o modelo linear simples, você realmente confiaria em suas previsões?

**Figura 1-23. Sobreajuste aos dados de treinamento**

Modelos complexos, como redes neurais profundas, podem detectar padrões sutis nos dados, mas se o conjunto de treinamento for ruidoso ou muito pequeno, o que introduz ruído de amostragem, o modelo provavelmente detectará padrões no próprio ruído (como no exemplo do motorista de táxi). Obviamente, esses padrões não generalizarão para novas instâncias. Por exemplo, digamos que você alimente seu modelo de satisfação com a vida com muitos atributos adicionais, incluindo alguns irrelevantes, como o nome do país. Nesse caso, um modelo complexo pode detectar padrões como o fato de que todos os países no conjunto de treinamento com um "w" no nome têm uma satisfação com a vida maior que 7: Nova Zelândia (7,3), Noruega (7,6), Suécia (7,3) e Suíça (7,5). Quão confiante você está de que a regra do "w-satisfação" generaliza para Ruanda ou Zimbábue? Obviamente, esse padrão ocorreu nos dados de treinamento por puro acaso, mas o modelo não tem como saber se um padrão é real ou simplesmente o resultado do ruído nos dados.

**AVISO**  
O sobreajuste ocorre quando o modelo é muito complexo em relação à quantidade e ao ruído dos dados de treinamento. Aqui estão algumas soluções possíveis:

- **Simplifique o modelo** selecionando um com menos parâmetros (por exemplo, um modelo linear em vez de um modelo polinomial de alto grau), reduzindo o número de atributos nos dados de treinamento ou restringindo o modelo.
- **Colete mais dados de treinamento**.
- **Reduza o ruído nos dados de treinamento** (por exemplo, corrija erros nos dados e remova outliers).

Restringir um modelo para torná-lo mais simples e reduzir o risco de sobreajuste é chamado de **regularização**. Por exemplo, o modelo linear que definimos anteriormente tem dois parâmetros, \(\theta_0\) e \(\theta_1\). Isso dá ao algoritmo de aprendizado dois **graus de liberdade** para ajustar o modelo aos dados de treinamento: ele pode ajustar tanto a altura (\(\theta_0\)) quanto a inclinação (\(\theta_1\)) da linha. Se forçarmos \(\theta_1 = 0\), o algoritmo terá apenas um grau de liberdade e terá muito mais dificuldade para ajustar os dados corretamente: tudo o que ele poderia fazer é mover a linha para cima ou para baixo para chegar o mais próximo possível das instâncias de treinamento, então ele acabaria em torno da média. Um modelo muito simples, de fato! Se permitirmos que o algoritmo modifique \(\theta_1\), mas o forçarmos a mantê-lo pequeno, o algoritmo de aprendizado terá efetivamente algo entre um e dois graus de liberdade. Ele produzirá um modelo mais simples do que um com dois graus de liberdade, mas mais complexo do que um com apenas um. Você quer encontrar o equilíbrio certo entre ajustar os dados de treinamento perfeitamente e manter o modelo simples o suficiente para garantir que ele generalize bem.

A Figura 1-24 mostra três modelos. A linha pontilhada representa o modelo original que foi treinado nos países representados como círculos (sem os países representados como quadrados), a linha sólida é nosso segundo modelo treinado com todos os países (círculos e quadrados), e a linha tracejada é um modelo treinado com os mesmos dados do primeiro modelo, mas com uma restrição de regularização. Você pode ver que a regularização forçou o modelo a ter uma inclinação menor: esse modelo não se ajusta tão bem aos dados de treinamento (círculos) quanto o primeiro modelo, mas na verdade generaliza melhor para novos exemplos que ele não viu durante o treinamento (quadrados).

**Figura 1-24. A regularização reduz o risco de sobreajuste**

A quantidade de regularização a ser aplicada durante o aprendizado pode ser controlada por um **hiperparâmetro**. Um hiperparâmetro é um parâmetro de um algoritmo de aprendizado (não do modelo). Como tal, ele não é afetado pelo algoritmo de aprendizado em si; ele deve ser definido antes do treinamento e permanece constante durante o treinamento. Se você definir o hiperparâmetro de regularização para um valor muito alto, obterá um modelo quase plano (uma inclinação próxima de zero); o algoritmo de aprendizado quase certamente não se ajustará excessivamente aos dados de treinamento, mas será menos provável que encontre uma boa solução. Ajustar hiperparâmetros é uma parte importante da construção de um sistema de aprendizado de máquina (você verá um exemplo detalhado no próximo capítulo).

---

#### **Subajuste aos Dados de Treinamento**

Como você pode imaginar, o **subajuste** é o oposto do sobreajuste: ocorre quando seu modelo é muito simples para aprender a estrutura subjacente dos dados. Por exemplo, um modelo linear de satisfação com a vida é propenso a subajuste; a realidade é simplesmente mais complexa do que o modelo, então suas previsões são inevitavelmente imprecisas, mesmo nos exemplos de treinamento.

Aqui estão as principais opções para corrigir esse problema:

- **Selecione um modelo mais poderoso**, com mais parâmetros.
- **Alimente melhores recursos ao algoritmo de aprendizado** (engenharia de recursos).
- **Reduza as restrições no modelo** (por exemplo, reduzindo o hiperparâmetro de regularização).

---

#### **Recapitulando**

Agora você já sabe muito sobre aprendizado de máquina. No entanto, passamos por tantos conceitos que você pode estar se sentindo um pouco perdido, então vamos recapitular e olhar para o quadro geral:

- **Aprendizado de máquina** é sobre fazer máquinas melhorarem em alguma tarefa aprendendo com dados, em vez de ter que codificar regras explicitamente.
- Existem muitos tipos diferentes de sistemas de ML: supervisionados ou não, em lote ou online, baseados em instância ou baseados em modelo.
- Em um projeto de ML, você coleta dados em um conjunto de treinamento e os alimenta a um algoritmo de aprendizado. Se o algoritmo for baseado em modelo, ele ajusta alguns parâmetros para ajustar o modelo ao conjunto de treinamento (ou seja, para fazer boas previsões no próprio conjunto de treinamento) e, em seguida, espera-se que ele faça boas previsões em novos casos também. Se o algoritmo for baseado em instância, ele simplesmente aprende os exemplos de cor e generaliza para novas instâncias usando uma medida de similaridade para compará-las às instâncias aprendidas.
- O sistema não terá um bom desempenho se o conjunto de treinamento for muito pequeno, ou se os dados não forem representativos, forem ruidosos ou estiverem poluídos com recursos irrelevantes (lixo entra, lixo sai). Por fim, seu modelo não deve ser nem muito simples (caso em que ele subajustará) nem muito complexo (caso em que ele sobreajustará).

Há apenas um último tópico importante a ser abordado: depois de treinar um modelo, você não quer apenas "esperar" que ele generalize para novos casos. Você quer avaliá-lo e ajustá-lo, se necessário. Vamos ver como fazer isso.

---

#### **Testando e Validando**

A única maneira de saber quão bem um modelo generalizará para novos casos é realmente testá-lo em novos casos. Uma maneira de fazer isso é colocar seu modelo em produção e monitorar seu desempenho. Isso funciona bem, mas se seu modelo for horrivelmente ruim, seus usuários reclamarão — não é a melhor ideia.

Uma opção melhor é dividir seus dados em dois conjuntos: o **conjunto de treinamento** e o **conjunto de teste**. Como os nomes sugerem, você treina seu modelo usando o conjunto de treinamento e o testa usando o conjunto de teste. A taxa de erro em novos casos é chamada de **erro de generalização** (ou **erro fora da amostra**), e, ao avaliar seu modelo no conjunto de teste, você obtém uma estimativa desse erro. Esse valor diz o quão bem seu modelo provavelmente se sairá em instâncias que nunca viu antes.

Se o erro de treinamento for baixo (ou seja, seu modelo comete poucos erros no conjunto de treinamento), mas o erro de generalização for alto, isso significa que seu modelo está sobreajustando os dados de treinamento.

**DICA**  
É comum usar 80% dos dados para treinamento e **reservar** 20% para teste. No entanto, isso depende do tamanho do conjunto de dados: se ele contiver 10 milhões de instâncias, reservar 1% significa que seu conjunto de teste terá 100.000 instâncias, provavelmente mais do que suficiente para obter uma boa estimativa do erro de generalização.

---

#### **Ajuste de Hiperparâmetros e Seleção de Modelos**

Avaliar um modelo é simples o suficiente: basta usar um conjunto de teste. Mas suponha que você esteja hesitando entre dois tipos de modelos (digamos, um modelo linear e um modelo polinomial): como você pode decidir entre eles? Uma opção é treinar ambos e comparar o quão bem eles generalizam usando o conjunto de teste.

Agora suponha que o modelo linear generalize melhor, mas você deseja aplicar alguma regularização para evitar o sobreajuste. A questão é: como você escolhe o valor do hiperparâmetro de regularização? Uma opção é treinar 100 modelos diferentes usando 100 valores diferentes para esse hiperparâmetro. Suponha que você encontre o melhor valor de hiperparâmetro que produz um modelo com o menor erro de generalização — digamos, apenas 5% de erro. Você lança esse modelo em produção, mas, infelizmente, ele não tem o desempenho esperado e produz 15% de erros. O que aconteceu?

O problema é que você mediu o erro de generalização várias vezes no conjunto de teste e adaptou o modelo e os hiperparâmetros para produzir o melhor modelo para aquele conjunto específico. Isso significa que o modelo provavelmente não terá um bom desempenho em novos dados.

Uma solução comum para esse problema é chamada de **validação holdout** (Figura 1-25): você simplesmente reserva parte do conjunto de treinamento para avaliar vários modelos candidatos e selecionar o melhor. O novo conjunto reservado é chamado de **conjunto de validação** (ou conjunto de desenvolvimento, ou dev set). Mais especificamente, você treina vários modelos com vários hiperparâmetros no conjunto de treinamento reduzido (ou seja, o conjunto de treinamento completo menos o conjunto de validação) e seleciona o modelo que tem o melhor desempenho no conjunto de validação. Após esse processo de validação holdout, você treina o melhor modelo no conjunto de treinamento completo (incluindo o conjunto de validação), e isso lhe dá o modelo final. Por fim, você avalia esse modelo final no conjunto de teste para obter uma estimativa do erro de generalização.

**Figura 1-25. Seleção de modelo usando validação holdout**

Essa solução geralmente funciona muito bem. No entanto, se o conjunto de validação for muito pequeno, as avaliações do modelo serão imprecisas: você pode acabar selecionando um modelo subótimo por engano. Por outro lado, se o conjunto de validação for muito grande, o conjunto de treinamento restante será muito menor do que o conjunto de treinamento completo. Por que isso é ruim? Bem, como o modelo final será treinado no conjunto de treinamento completo, não é ideal comparar modelos candidatos treinados em um conjunto de treinamento muito menor. Seria como selecionar o corredor mais rápido para participar de uma maratona. Uma maneira de resolver esse problema é realizar **validação cruzada repetida**, usando muitos pequenos conjuntos de validação. Cada modelo é avaliado uma vez por conjunto de validação depois de ser treinado no restante dos dados. Ao calcular a média de todas as avaliações de um modelo, você obtém uma medida muito mais precisa de seu desempenho. No entanto, há uma desvantagem: o tempo de treinamento é multiplicado pelo número de conjuntos de validação.

---

#### **Desajuste de Dados**

Em alguns casos, é fácil obter uma grande quantidade de dados para treinamento, mas esses dados provavelmente não serão perfeitamente representativos dos dados que serão usados em produção. Por exemplo, suponha que você queira criar um aplicativo móvel para tirar fotos de flores e determinar automaticamente suas espécies. Você pode facilmente baixar milhões de fotos de flores na web, mas elas não serão perfeitamente representativas das fotos que realmente serão tiradas usando o aplicativo em um dispositivo móvel. Talvez você tenha apenas 1.000 fotos representativas (ou seja, realmente tiradas com o aplicativo).

Nesse caso, a regra mais importante a lembrar é que tanto o conjunto de validação quanto o conjunto de teste devem ser o mais representativos possível dos dados que você espera usar em produção, então eles devem ser compostos exclusivamente de fotos representativas: você pode embaralhá-las e colocar metade no conjunto de validação e metade no conjunto de teste (garantindo que nenhuma duplicata ou quase-duplicata acabe em ambos os conjuntos). Depois de treinar seu modelo nas fotos da web, se você observar que o desempenho do modelo no conjunto de validação é decepcionante, você não saberá se isso ocorre porque seu modelo sobreajustou o conjunto de treinamento ou se é apenas devido ao desajuste entre as fotos da web e as fotos do aplicativo móvel.

Uma solução é reservar algumas das fotos de treinamento (da web) em outro conjunto que Andrew Ng chamou de **conjunto train-dev** (Figura 1-26). Depois que o modelo for treinado (no conjunto de treinamento, não no conjunto train-dev), você pode avaliá-lo no conjunto train-dev. Se o modelo tiver um desempenho ruim, ele deve ter sobreajustado o conjunto de treinamento, então você deve tentar simplificar ou regularizar o modelo, obter mais dados de treinamento e limpar os dados de treinamento. Mas se ele tiver um bom desempenho no conjunto train-dev, você pode avaliar o modelo no conjunto de validação. Se ele tiver um desempenho ruim, o problema deve estar vindo do desajuste de dados. Você pode tentar resolver esse problema pré-processando as imagens da web para que pareçam mais com as fotos que serão tiradas pelo aplicativo móvel e, em seguida, retreinar o modelo. Uma vez que você tenha um modelo que tenha um bom desempenho tanto no conjunto train-dev quanto no conjunto de validação, você pode avaliá-lo uma última vez no conjunto de teste para saber o quão bem ele provavelmente se sairá em produção.

**Figura 1-26. Quando os dados reais são escassos (direita), você pode usar dados abundantes semelhantes (esquerda) para treinamento e reservar alguns deles em um conjunto train-dev para avaliar o sobreajuste; os dados reais são então usados para avaliar o desajuste de dados (conjunto de validação) e o desempenho do modelo final (conjunto de teste)**

---

#### **Teorema do Almoço Grátis**

Um modelo é uma representação simplificada dos dados. As simplificações destinam-se a descartar os detalhes supérfluos que provavelmente não generalizarão para novas instâncias. Quando você seleciona um tipo específico de modelo, está implicitamente fazendo suposições sobre os dados. Por exemplo, se você escolher um modelo linear, está implicitamente assumindo que os dados são fundamentalmente lineares e que a distância entre as instâncias e a linha reta é apenas ruído, que pode ser ignorado com segurança.

Em um famoso artigo de 1996, David Wolpert demonstrou que, se você não fizer absolutamente nenhuma suposição sobre os dados, não há razão para preferir um modelo sobre qualquer outro. Isso é chamado de **Teorema do Almoço Grátis (NFL)**. Para alguns conjuntos de dados, o melhor modelo é um modelo linear, enquanto para outros é uma rede neural. Não há modelo que seja **a priori** garantido para funcionar melhor (daí o nome do teorema). A única maneira de saber com certeza qual modelo é o melhor é avaliar todos eles. Como isso não é possível, na prática você faz algumas suposições razoáveis sobre os dados e avalia apenas alguns modelos razoáveis. Por exemplo, para tarefas simples, você pode avaliar modelos lineares com vários níveis de regularização, e para um problema complexo, você pode avaliar várias redes neurais.

---

#### **Exercícios**

Neste capítulo, cobrimos alguns dos conceitos mais importantes do aprendizado de máquina. Nos próximos capítulos, mergulharemos mais fundo e escreveremos mais código, mas antes disso, certifique-se de que você pode responder às seguintes perguntas:

1. Como você definiria aprendizado de máquina?
2. Você pode nomear quatro tipos de aplicações onde ele se destaca?
3. O que é um conjunto de treinamento rotulado?
4. Quais são as duas tarefas supervisionadas mais comuns?
5. Você pode nomear quatro tarefas não supervisionadas comuns?
6. Que tipo de algoritmo você usaria para permitir que um robô andasse em vários terrenos desconhecidos?
7. Que tipo de algoritmo você usaria para segmentar seus clientes em vários grupos?
8. Você enquadraria o problema de detecção de spam como um problema de aprendizado supervisionado ou não supervisionado?
9. O que é um sistema de aprendizado online?
10. O que é aprendizado fora do núcleo?
11. Que tipo de algoritmo depende de uma medida de similaridade para fazer previsões?
12. Qual é a diferença entre um parâmetro do modelo e um hiperparâmetro do modelo?
13. O que os algoritmos baseados em modelo procuram? Qual é a estratégia mais comum que eles usam para ter sucesso? Como eles fazem previsões?
14. Você pode nomear quatro dos principais desafios no aprendizado de máquina?
15. Se o seu modelo tiver um ótimo desempenho nos dados de treinamento, mas generalizar mal para novas instâncias, o que está acontecendo? Você pode nomear três soluções possíveis?
16. O que é um conjunto de teste e por que você o usaria?
17. Qual é o propósito de um conjunto de validação?
18. O que é um conjunto train-dev, quando você precisa dele e como o usa?
19. O que pode dar errado se você ajustar hiperparâmetros usando o conjunto de teste?

As soluções para esses exercícios estão disponíveis no final do notebook deste capítulo, em https://homl.info/colab3.

---

#### **Notas de Rodapé**

1. Curiosidade: esse nome estranho é um termo estatístico introduzido por Francis Galton enquanto ele estudava o fato de que os filhos de pessoas altas tendem a ser mais baixos que seus pais. Como os filhos eram mais baixos, ele chamou isso de **regressão à média**. Esse nome foi então aplicado aos métodos que ele usou para analisar correlações entre variáveis.
2. Observe como os animais estão bem separados dos veículos e como os cavalos estão próximos dos cervos, mas longe dos pássaros. Figura reproduzida com permissão de Richard Socher et al., "Zero-Shot Learning Through Cross-Modal Transfer", *Proceedings of the 26th International Conference on Neural Information Processing Systems* 1 (2013): 935–943.
3. Isso é quando o sistema funciona perfeitamente. Na prática, ele frequentemente cria alguns clusters por pessoa e, às vezes, mistura duas pessoas que se parecem, então você pode precisar fornecer alguns rótulos por pessoa e limpar manualmente alguns clusters.
4. Por convenção, a letra grega \(\theta\) (theta) é frequentemente usada para representar parâmetros do modelo.
5. Tudo bem se você não entender todo o código ainda; apresentarei o Scikit-Learn nos próximos capítulos.
6. Por exemplo, saber se deve escrever "to", "two" ou "too", dependendo do contexto.
7. Peter Norvig et al., "The Unreasonable Effectiveness of Data", *IEEE Intelligent Systems* 24, no. 2 (2009): 8–12.
8. Figura reproduzida com permissão de Michele Banko e Eric Brill, "Scaling to Very Very Large Corpora for Natural Language Disambiguation", *Proceedings of the 39th Annual Meeting of the Association for Computational Linguistics* (2001): 26–33.
9. David Wolpert, "The Lack of A Priori Distinctions Between Learning Algorithms", *Neural Computation* 8, no. 7 (1996): 1341–1390.

---






