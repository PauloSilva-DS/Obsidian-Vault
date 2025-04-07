---
title: How I created UI with ChatGPT’s new image generator (4o)
url: https://medium.com/design-bootcamp/how-i-created-ui-with-chatgpts-new-image-generator-4o-d52389a5833e
author:
  - "[[Xinran Ma]]"
published: 2025-03-31
data: 2025-04-07T14:39:21-03:00
description: GPT-4o’s new image generator goes beyond Ghibli-style art—see how it creates effective UI mockups for product designers, product managers, and UX pros.
tags:
  - clippings
Atualizado: 2025-04-07  14.39
Criado: 2025-04-07  14.39
---
Prompts, orientações e surpresas

***É possível que o ChatGPT gere interface de usuário?***

Eu testei isso várias vezes antes, mas os resultados foram muito decepcionantes.

Abaixo está um exemplo que compartilhei em [**um boletim informativo**](https://designwithai.substack.com/p/claude-vs-chatgpt-for-designers) há dois meses.

Gerado via ChatGPT antes da atualização

Parecia muito caricatural para ser utilizável.

No entanto, na semana passada, a OpenAI lançou uma atualização importante, então decidi tentar novamente.

Consegui gerar mockups de UI muito melhores com base em meus prompts. Pude até criar várias opções de design para minhas necessidades:

Gerado via ChatGPT

Hoje, mostrarei os experimentos que realizei, os prompts que usei e algumas surpresas e aprendizados ao longo do caminho.

Vamos mergulhar.

## O Contexto

A OpenAI anunciou que agora é possível gerar imagens de alta qualidade no ChatGPT usando GPT-4o, em vez do antigo modelo DALL·E.

Ele segue melhor as instruções e renderiza melhor o texto em imagens.

Muitas pessoas testaram na semana passada a transformação de fotos em arte de IA — e é por isso que a tendência de arte no estilo Ghibli se tornou viral.

Eu também tentei:

Gerado via ChatGPT

Mas depois de ver muitas artes interessantes online, pensei:

==*E se eu pudesse usar o ChatGPT para gerar uma interface de usuário realmente útil para um designer de produto?*==

Foi aí que os experimentos começaram.

## Os Experimentos

## 1\. Crie um prompt detalhado

Usei o ChatGPT para me ajudar a gerar um prompt detalhado para a IU que eu queria.

```c
# Crie uma tela de UI móvel limpa e moderna (estilo iOS) para um aplicativo intitulado "SkillVerse – Trending Micro-Courses". O layout deve seguir as seções estruturadas abaixo.## 1. Barra de status (superior)- **Estilo**: Layout padrão do iOS (área segura superior)---## 2. Seção de Cabeçalho- **Logotipo centralizado**: \`SkillVerse\`  - **Fonte**: Peso médio, tamanho pequeno  - **Cor**: Texto azul  ---## 3. Carrossel de cursos em destaque (rolável horizontalmente)- **Estilo**: Cartões deslizantes com cantos arredondados e sombra suave  - **Cartas**:    - **Cartão 1**      - Título: **Introdução ao Motion Design**      - Legenda: *Começa em 2 de abril*      - Visual: Miniatura de animação    - **Cartão 2**      - Título: **Dominando o Excel para Freelancers**      - Legenda: *Começa em 31 de março*      - Visual: Ícone de produtividade  ---## 4. Guias de navegação- **Guias**:    - **Tendências** (Ativo, **rótulo em negrito** com **sublinhado azul**)    - Recomendado    - Matriculado    - Salvo  ---## 5. Filtrar linha- **Filtros (menus suspensos)**:    - **Últimos 7 dias** (com base no tempo)    - **Todos os tópicos** (categoria)    - **Todos os níveis** (dificuldade)  ---## 6. Lista de cursos em alta- **Layout**: Pilha vertical de itens de curso repetíveis:  - **Esquerda**: Miniatura arredondada    - **Centro**:      - Título do curso      - Nível (por exemplo, iniciante, intermediário, etc.)    - **Direita**: Ícone Salvar  - **Inferior**: Contagem de matrículas + tendência (por exemplo, 2,4 mil matriculados, +12%)   ---## 7. Barra de navegação inferior- **Guias**:  - **Home** (Ativo, cor destacada)    - Procurar    - Eventos    - Perfil  - **Estilo**:    - Etiquetas embaixo
```

Depois colei em uma nova janela de bate-papo no ChatGPT e cliquei no ícone “Gerar”.

## 2\. Uma IU com suporte de código gerada

Surpreendentemente, o ChatGPT abriu um painel extra à direita, acionando seu recurso Canvas. Então, ele começou a gerar código.

Isso imediatamente me lembrou do recurso Artefato do Claude.

Captura de tela do ChatGPT

Então cliquei no botão “Visualizar” no canto superior direito.

Foi gerada uma interface de usuário responsiva e com suporte de código.

Captura de tela do ChatGPT

Foi uma delícia de assistir, mas também pareceu menos preciso/polido que Claude.

## 3\. Peça um modelo visual em vez disso

Como minha intenção era simplesmente gerar uma imagem (um modelo visual) em vez de uma interface de usuário baseada em código, solicitei um prompt de acompanhamento para corrigi-lo:

```c
Em vez disso, crie um modelo visual.
```

Aqui está o resultado:

Gerado via ChatGPT

Foi surpreendentemente bom, especialmente comparado ao que o ChatGPT (DALL·E) era capaz de gerar antes. Uma grande atualização.

*Quiz: Quantos erros de digitação você consegue encontrar na interface de usuário gerada pela IA acima?:)*

## 4\. Peça para reduzir a escala da IU

Embora a interface parecesse boa, ainda fiquei um pouco incomodado com o corte da imagem, então perguntei:

```c
A parte superior e inferior parecem um pouco cortadas. Você poderia reduzir a UI em cerca de 20%  para torná-la menor no geral?
```

Aqui está o resultado:

Gerado via ChatGPT

Ficou lindo!

## 5\. Peça para gerar opções de design

Então comecei a ficar mais criativo… Eu me perguntei, e se eu pedisse para ele gerar múltiplas opções de design para eu considerar? Isso seria ainda mais útil!

Então eu escrevi isso:

```c
Crie três opções de design.Use o mesmo conteúdo principal, mas varie o layout, os elementos da interface do usuário e a ênfase visual de acordo com as seguintes descrições:Opção 1. Ousado e envolvente- Priorizar o impacto visual e a expressão da marcaOpção 2. Funcional e rápido- Priorize rapidez, clareza e produtividade- Interface compacta com cartões menores e hierarquia de informações densaOpção 3. Personalizado e Aconchegante- Priorize a conexão, a confiança e a personalização- Carrossel “Para você” com sugestões de cursos selecionadas por IA- Prova social (avatares, emblemas, atividade de amigos)- Adicione uma aba “Comunidade” para conteúdo compartilhado por pares
```

Aqui está o resultado:

Gerado via ChatGPT

Mais uma vez, fiquei chocado com a melhoria na geração de imagens do GPT-4o.

Embora as três opções ainda pareçam bastante semelhantes, se você olhar atentamente, há pequenos detalhes — como o ícone “Pesquisar”, prova social e texto — que tentam tornar cada versão diferente das outras.

O resultado certamente tinha espaço para melhorias, no entanto. Por exemplo, como há muitas informações compactadas em uma imagem, o ChatGPT começou a ter dificuldades com precisão. Você pode notar que parte do texto ficou irreconhecível/distorcida.

## 6\. Converta os designs para arquivo Figma

Em seguida, como um teste divertido, usei [o plugin Codia AI](https://www.figma.com/community/plugin/1329812760871373657/codia-ai-design-screenshot-to-editable-figma-design) para gerar designs Figma com base nos mockups visuais gerados pelo ChatGPT.

Captura de tela no Figma

Todos os componentes, incluindo o texto, eram editáveis no Figma.

E a família de fontes usada era Intel.

Assustadoramente bom.

Isso me dá a capacidade de personalizar modelos de interface do usuário, caso eu queira fazer alterações.

## Considerações finais

A capacidade do ChatGPT de gerar mockups visuais de UI é uma grande atualização. Ele oferece muito mais precisão e aderência aos prompts em comparação a antes.

Dito isso, a velocidade é um pouco lenta, e a precisão ainda pode ser melhorada. Às vezes, a imagem para de gerar na metade; às vezes, os resultados são aleatórios e não seguem as instruções de perto.

Quando pedi ao ChatGPT para criar um modelo 3D com base nas opções de design, as coisas ficaram um pouco distorcidas e estranhas — mas não tão ruins.

Gerado via ChatGPT

De qualquer forma, já é uma ótima atualização. Agora consigo gerar coisas que não conseguia antes.

E tudo acontece dentro da janela de bate-papo do ChatGPT. Quão conveniente é isso?

Estou muito animado e esperançoso por atualizações ainda melhores no futuro.

Obrigado pela leitura!

Vejo vocês na semana que vem.

Xinran

📮 Junte-se a mim no [**Design with AI**](https://designwithai.co/), uma publicação digital que explora o potencial da IA no design. Depois de assinar, você receberá artigos práticos toda semana para ajudar você a projetar melhor, mais rápido e de forma mais inteligente com IA. Você também se tornará parte de uma comunidade com mais de 4 mil entusiastas de IA de empresas como Google e Amazon.

🎉 Confira as novas turmas do nosso curso de IA: [**IA para Designers de Produto**](https://maven.com/xinran/ai-for-product-designers)!