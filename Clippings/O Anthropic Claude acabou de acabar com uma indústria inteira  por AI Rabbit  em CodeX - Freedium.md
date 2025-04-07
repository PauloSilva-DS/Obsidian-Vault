---
title: O Anthropic Claude acabou de acabar com uma indústria inteira? | por AI Rabbit | em CodeX - Freedium
url: https://freedium.cfd/https://medium.com/codex/has-anthropic-claude-just-wiped-out-an-entire-industry-fc1ada7a8d91
author: 
published: 
data: 2025-04-07T14:34:10-03:00
description: If you have been following the news, you may have read about a new feature (or should I call it a...
tags:
  - clippings
Atualizado: 2025-04-07  14.34
Criado: 2025-04-07  14.34
---
[< Ir para o original](https://medium.com/codex/has-anthropic-claude-just-wiped-out-an-entire-industry-fc1ada7a8d91#bypass)

![Imagem de pré-visualização](https://miro.medium.com/v2/resize:fit:700/0*vx14ZsK32TV7BWTG.png)

## O Claude Antrópico acabou com uma indústria inteira?

## Se você tem acompanhado as notícias, você pode ter lido sobre um novo recurso (ou devo chamá-lo de produto) na API Claude — é…

[Coelho AI](https://medium.com/@airabbitX "Entusiasta de IA e geek de produtividade") [![Códice](https://miro.medium.com/v2/resize:fill:48:48/1*VqH0bOrfjeUkznphIC7KBg.png)

Códice

](https://medium.com/codex "Tudo relacionado a Tecnologia e Código.")estúdio android ~8 min de leitura · 27 de outubro de 2024 (Atualizado: 2 de novembro de 2024) · Grátis: Não

Se você tem acompanhado as notícias, talvez tenha lido sobre um novo recurso (ou devo chamá-lo de produto) na API do Claude — ele se chama **Uso do Computador**.

Essa tecnologia não é apenas mais um recurso bacana do Claude, mas pode ser o começo do fim para muitas indústrias. Mas antes de olharmos para as implicações, vamos dar uma olhada no que o Computer Use realmente é.

Aqui está o anúncio oficial da Anthropic sobre o uso do computador:

Apresentando o uso do computador, um novo Soneto Claude 3.5 e Haiku Claude 3.5

### Um Claude 3.5 Sonnet atualizado e mais poderoso, um Claude 3.5 Haiku e uma nova capacidade experimental de IA: uso de computador.

antrópico.com

[View original](https://www.anthropic.com/news/3-5-models-and-computer-use)

Mas antes de nos aprofundarmos nisso e em suas implicações, tenho que mencionar que controlar computadores com IA não é realmente algo novo. Houve muitos projetos que foram nessa direção usando agentes.

Alguns dos projetos de código aberto que tenho nos meus favoritos do Github:

[https://github.com/e2b-dev/awesome-ai-agents](https://github.com/e2b-dev/awesome-ai-agents)

[https://github.com/xlang-ai/OSWorld](https://github.com/xlang-ai/OSWorld)

Esses projetos têm muito potencial para tornar essa tecnologia disponível para todos, mesmo no local, usando modelos locais (a solução Claude é de código fechado e atualmente depende muito de modelos antrópicos).

Notícias do coelho AI

### Descobrindo o que há de mais recente em IA: tendências, ferramentas e tutoriais

blogue airabbit

[View original](https://airabbit.blog/)

No entanto, como é frequentemente o caso, o desafio está na execução. A arquitetura desses agentes pode ser semelhante, mas tudo se resume às **capacidades de raciocínio** do modelo.

> *Se o modelo consegue raciocinar bem, então ele provavelmente consegue executar as tarefas que lhe são solicitadas com pouca ou nenhuma intervenção do usuário.*

**Parece bom? Vamos dar uma olhada:**

### Como funciona

A ideia é simples:

1. Primeiro, um cliente (como um aplicativo Python) recebe comandos do usuário e os passa para Claude junto com uma captura de tela do ambiente de trabalho.
2. Então Claude interpreta o comando e lê a imagem com as informações da área de trabalho para determinar qual ação tomar com base no estado atual da área de trabalho. O cliente então interpreta os comandos de controle de Claude e realmente executa esses comandos com movimentos e cliques simulados do mouse, agindo como um humano faria.

Observe, no entanto, que **Claude não** **executa** (e não pode executar) essas instruções; ele simplesmente as retorna como tokens de saída para o chamador (o aplicativo Python neste caso), e o aplicativo é responsável por executá-las na máquina host (Linux, Mac, qualquer que seja).

Aqui está um aplicativo cliente de início rápido fornecido pela Anthropic que usa uma VM com Docker para simular esse comportamento. Como você pode ver, a tela é dividida em duas partes. À esquerda, você pode conversar e dar instruções, e à direita, você pode ver o comportamento real do robô.

Se você quiser experimentar, é super simples com o Docker.

> Há duas coisas que sugiro que você considere ao escolher **qual modelo** usar (Anthropic Claude vs Bedrock Claude):

> O modelo Anthropic tem **cache de Contexto,** o que é ótimo para economizar dinheiro no custo dos tokens de entrada! No entanto, ele tem alguns limites, então tome cuidado para não atingi-los se for além de alguns cenários de teste simples.

> O AWS Bedrock é quase o oposto exato! Os **limites são muito mais altos** (eu nunca os atingi de fato), mas a desvantagem é que ele **não tem cache**, então você pagará mais pela mesma tarefa do que o Anthropic Claude.

[https://github.com/anthropics/anthropic-quickstarts/blob/main/computer-use-demo/README.md](https://github.com/anthropics/anthropic-quickstarts/blob/main/computer-use-demo/README.md)

```bash
docker run \
 -e API_PROVIDER=bedrock \
 -e AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID \
 -e AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY \
 -e AWS_REGION=us-west-2 \
 -v $HOME/.anthropic:/home/computeruse/.anthropic \
 -p 5900:5900 \
 -p 8501:8501 \
 -p 6080:6080 \
 -p 8080:8080 \
 -it ghcr.io/anthropics/anthropic-quickstarts:computer-use-demo-latest
```
Notícias do coelho AI

### Explore notícias, tendências, ferramentas e tutoriais sobre IA.

blogue airabbit

[View original](https://airabbit.blog/)

### Principais avanços

Já experimentei um pouco com RPA no passado e, mais recentemente, com o Computer Use da Anthropic e, na minha opinião, há duas diferenças principais:

**Primeiro**, ao contrário de alguns RPAs que são adaptados para cenários específicos, **o Uso do Computador** é 100% genérico e pode funcionar com qualquer aplicativo e em qualquer ambiente (se você tiver um mouse e um cliente que controle o sistema operacional).

**Em segundo lugar**, as soluções genéricas anteriores não tinham capacidades de raciocínio suficientes para executar tarefas complexas envolvendo muitas etapas e, mais importante, não eram capazes de se recuperar quando algo inesperado acontecia. Com o Computer Use em Claude, isso parece ter melhorado significativamente.

Eu testei com alguns cenários complexos e ele conseguiu executá-los muito bem.

### Limitações

Apesar do enorme potencial desta tecnologia, há algumas limitações das quais precisamos estar cientes.

**As limitações do token**

Dependendo da duração da tarefa, você pode atingir um limite de token diário ou por minuto ao usar a API Anthropic. Tenha em mente que a conversa não é apenas texto, mas seu cliente deve enviar continuamente uma captura de tela da sua área de trabalho, o que aumenta drasticamente o número de tokens usados.

> *Observe que os limites são muito maiores se você estiver usando Bedrock em vez da Anthropic API. Mais informações sobre os limites atuais podem ser encontradas no site do provedor.*

**Custo**

Dependendo da tarefa que você deseja executar, você também precisará considerar o custo. Assim como os limites de token, eles aumentam massivamente com o número de capturas de tela que você tira. Claro, isso depende muito do modelo que você usa e do cache que você faz.

> *Observe que você pode economizar muito dinheiro usando* ***o cache de contexto****, que atualmente só está disponível se você usar a API Anthropic diretamente, não por meio do AWS Bedrock. O cache de contexto economiza até 90% do custo de tokens de entrada, então vale a pena considerar se o limite de tokens não é um problema.*

> *Para mais informações sobre preços atuais e preços de cache de contexto, visite o site da Anthropic.*

**Velocidade de execução**

Usar o Computer Use requer muita interação multimídia com a API Claude, incluindo a transferência de imagens. Isso tem um impacto significativo na largura de banda, mas também na velocidade de execução dessas tarefas. Para tarefas recorrentes que são altamente estruturadas e podem ser manipuladas por uma API, usar o Computer Use não seria a solução.

**Guarda-corpos Antrópicos**

O Anthropic tem proteções rígidas em seus modelos e não permite todas as ações solicitadas pelo usuário. Por exemplo, ele pode não fazer login em um site se você fornecer um nome de usuário e uma senha. Ele também pode relutar em abrir certos aplicativos ou executar certas ações em alguns sites ou aplicativos. Isso pode fazer ou quebrar seu caso de uso se você estiver contando com uma solução totalmente autônoma devido à sua imprevisibilidade (pode funcionar uma vez e falhar em outra). Por esse motivo, um **humano no circuito** pode ser necessário neste estágio de desenvolvimento.

**Preocupações com privacidade e segurança**

Ter alguém fazendo o trabalho sujo para você é ótimo, mas dá a esse software uma quantidade incrível de poder. A quantidade de informações que ele (e a Anthropic como empresa, é claro) pode monitorar e a quantidade de controle que ele tem sobre seu dispositivo é imensa. Isso tem que ser levado em conta, especialmente em cenários onde o computador está realmente agindo em seu nome, como enviar informações (talvez até mesmo informações confidenciais).

### O impacto na indústria

Como mencionado acima, ainda há muitos desafios a serem superados antes que essa tecnologia possa se desenvolver e começar a perturbar muitas partes da indústria. E, claro, não é apenas a Anthropic que está trabalhando nisso, mas a OpenAI e muitas outras empresas.

**O controle totalmente autônomo** de software sempre foi o **Santo Graal da robótica** e da IA devido ao imenso potencial de economia de custos, tanto para empresas quanto para uso pessoal.

No entanto, dado o atual estado de desenvolvimento e pensamento, e os esforços contínuos para tornar essa tecnologia mais barata e rápida, o impacto na economia pode ser enorme e fazer com que muitas tecnologias surjam e tecnologias e empresas ou mesmo indústrias existentes sejam extintas mais rápido do que você imagina.

Posso pensar em alguns produtos que seriam obsoletos, mas vamos ver o que o GPT prevê:

```bash
For sure! The "computer use" feature introduced by Anthropic's Claude API represents a significant leap in AI capabilities, particularly in automating tasks that traditionally require human intervention. Here are a few examples of how this technology could impact different industries:
Administrative and back-office operations:
Impact: Automation of routine tasks such as data entry, scheduling and document management.
Result: Increased efficiency and reduced operating costs, potentially leading to a reduction in the need for administrative staff.
Customer service and support:
Impact: AI agents can handle customer queries, troubleshoot issues and perform account actions without human intervention.
Result: Improved customer experience with 24/7 support and faster resolution times.
Software testing and quality assurance:
Impact: Automate the testing process by simulating user interactions with applications to identify bugs and performance issues.
Result: Faster development cycles and improved software reliability.
Financial Services:
Impact: Automated transaction processing, compliance checks and risk assessment.
Result: More efficient financial operations and reduced risk of human error.
Healthcare administration:
Impact: Manage patient records, schedule appointments and process insurance claims automatically.
Result: Streamlined administrative processes, allowing healthcare professionals to focus more on patient care.
E-commerce and retail:
Impact: Automatically manage inventory, update product listings and process customer orders.
Result: Optimised supply chain and inventory management, resulting in cost savings and improved customer satisfaction.
Human Resources:
Impact: Automated recruitment processes, employee onboarding and payroll management.
Result: Improved efficiency in HR operations and a more seamless experience for employees.
Training and e-learning platforms:
Impact: Personalised learning experiences by automating content delivery based on student performance and interactions.
Results: Improved educational outcomes through customised learning paths.
Legal Services:
Impact: Automated document analysis, contract review and compliance monitoring.
Result: Reduced workload for legal professionals and faster turnaround times for clients.
Marketing and content creation:
Impact: Automated campaign management, social media interactions and content planning.
Result: A more consistent brand presence and the ability to scale marketing efforts without a proportional increase in staff.
Overall Impact:
Workforce transformation: There could be a significant shift in job requirements, with more demand for roles in AI oversight and fewer in routine task execution.
Cost Reduction: Businesses could see lower operating costs, with savings potentially passed on to consumers.
Increased productivity: Automation of mundane tasks could lead to increased productivity, allowing human workers to focus on strategic initiatives.
Security and ethical considerations: As AI performs more actions autonomously, ensuring data security and ethical use becomes paramount.
Accelerating innovation: Businesses can innovate faster, with the ability to quickly prototype and test new ideas using AI.
Potential challenges:
Job displacement: Some positions may become obsolete, requiring retraining and education programmes for affected workers.
Regulatory hurdles: Rapid adoption could outpace existing regulations, leading to legal and compliance challenges.
Dependence on AI providers: Heavy reliance on technology from companies such as Anthropic could lead to vendor lock-in scenarios.
Conclusion:
The 'computer use' function has the potential to revolutionise multiple industries by enabling AI to perform complex tasks autonomously. While there are many benefits in terms of efficiency and productivity, there are also challenges that need to be carefully addressed to maximise positive outcomes and mitigate risks.
```

Incrível e assustador ao mesmo tempo.

**Embrulhe-o**.

Esta nova tecnologia, na minha opinião, superou uma enorme barreira e acredito que é apenas uma questão de tempo até que ela libere todo o seu potencial e talvez vire a economia de cabeça para baixo, como o ChatGPT fez alguns anos atrás.