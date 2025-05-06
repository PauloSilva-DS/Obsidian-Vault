---
title: "Qwen3: Recursos, comparação do DeepSeek-R1, acesso e muito mais"
url: "https://www.datacamp.com/pt/blog/qwen3"
author:
published:
data: "2025-05-05T13:25:23-03:00"
description: "Saiba mais sobre a suíte Qwen3, incluindo sua arquitetura, implementação e benchmarks em comparação com o DeepSeek-R1 e o Gemini 2.5 Pro."
tags:
  - "clippings"
---
[

##### Formação ilimitada com 50% de desconto

](https://www.datacamp.com/pt/promo/learn-data-and-ai-may-25)

[Pular para o conteúdo principal](https://www.datacamp.com/pt/blog/#main)

**O Qwen3** é uma das mais completas suítes de modelos de peso aberto lançadas até o momento.

Ele vem da equipe Qwen da Alibaba e inclui modelos que aumentam o desempenho em nível de pesquisa, bem como versões menores que podem ser executadas localmente em hardware mais modesto.

Neste blog, apresentarei uma rápida visão geral do conjunto completo do Qwen3, explicarei como os modelos foram desenvolvidos, analisarei os resultados de benchmark e mostrarei como você pode acessá-los e começar a usá-los.

Nossa equipe também está trabalhando em tutoriais que mostram como executar o Qwen3 localmente e como ajustar os modelos do Qwen3. Certificar-me-ei de atualizar este artigo assim que eles estiverem prontos, portanto, se você voltar aqui nos próximos dois ou três dias, encontrará links para esses recursos adicionados nesta introdução.

Mantemos nossos leitores atualizados sobre as últimas novidades em IA enviando o The Median, nosso boletim informativo gratuito de sexta-feira que detalha as principais histórias da semana. Inscreva-se e fique atento em apenas alguns minutos por semana:

## O que é o Qwen 3?

O Qwen3 é a mais recente família de modelos de idiomas grandes da equipe Qwen da Alibaba. Todos os modelos da linha são abertos sob a licença Apache 2.0.

O que me chamou a atenção de imediato foi a introdução de um orçamento de pensamento que os usuários podem controlar diretamente no aplicativo Qwen. Isso proporciona aos usuários comuns um controle granular sobre o processo de raciocínio, algo que antes só podia ser feito de forma programática.

![qwen 3 pensando no orçamento](https://media.datacamp.com/cms/ad_4nxfufg-zwgvofgyo4ava9b7o5ysdk41hiwoqpmh27fceho7uh3hofpbozw4hy9vckb5owwp5xs0_iihmi_h7a_9l-rqaww69wptninr7blaplkh45ejpaqgnkvkv5_f8lk2rmvgpna.png)

Como podemos ver nos gráficos abaixo, aumentar os orçamentos de raciocínio melhora significativamente o desempenho, especialmente em matemática, codificação e ciências.

![qwen 3 orçamento pensante melhora o desempenho](https://media.datacamp.com/cms/ad_4nxc4bh5x3cd5niixevbxaavmmo5sp9-soav-b_-zs-pdwjgdbiwzwsbw0qqxe6yqfxu8qlbo5w8hurqbylvzrhvly8klkgtizynvt-o8pk--ealw5qfeiohpgu1oqabcjfl7stiiog.png)

Fonte: [Qwen](https://qwenlm.github.io/blog/qwen3/)

Nos testes de benchmark, o carro-chefe Qwen3-235B-A22B tem um desempenho competitivo em relação a outros modelos de primeira linha e apresenta resultados mais fortes do que o [DeepSeek-R1](https://www.datacamp.com/pt/blog/deepseek-r1) em codificação, matemática e raciocínio geral. Vamos explorar rapidamente cada modelo e entender para que ele foi projetado.

### Qwen3-235B-A22B

Esse é o maior modelo da linha Qwen3. Ele usa uma [mistura de especialistas (MoE)](https://www.datacamp.com/pt/blog/mixture-of-experts-moe) com 235 bilhões de parâmetros totais e 22 bilhões ativos por etapa de geração.

Em um modelo MoE, apenas um pequeno subconjunto de parâmetros é ativado em cada etapa, o que torna sua execução mais rápida e econômica em comparação com modelos densos (como o GPT-4o), em que todos os parâmetros são sempre usados.

O modelo tem bom desempenho em tarefas de matemática, raciocínio e codificação e, em comparações de benchmark, supera modelos como o DeepSeek-R1.

### Qwen3-30B-A3B

O Qwen3-30B-A3B é um modelo MoE menor, com 30 bilhões de parâmetros totais e apenas 3 bilhões ativos em cada etapa. Apesar do baixo número de ativos, seu desempenho é comparável ao de modelos densos muito maiores, como o [QwQ-32B](https://www.datacamp.com/pt/blog/qwq-32b). É uma opção prática para usuários que desejam uma combinação de capacidade de raciocínio e custos de inferência mais baixos. Assim como o modelo 235B, ele suporta uma janela de contexto de 128K e está disponível no Apache 2.0.

### Modelos densos: 32B, 14B, 8B, 4B, 1,7B, 0,6B

Os seis modelos densos na versão Qwen3 seguem uma arquitetura mais tradicional em que todos os parâmetros estão ativos em cada etapa. Eles abrangem uma ampla gama de casos de uso:

Qwen3-32B, 14B, 8B suportam janelas de contexto de 128K, enquanto Qwen3-4B, 1.7B, 0.6B suportam 32K. Todos são abertos e licenciados sob o Apache 2.0. Os modelos menores desse grupo são adequados para implementações leves, enquanto os maiores estão mais próximos dos LLMs de uso geral.

### Qual modelo você deve escolher?

O Qwen3 oferece diferentes modelos, dependendo da profundidade de raciocínio, da velocidade e do custo computacional que você precisa. Aqui está uma rápida visão geral do site :

| Modelo | Tipo | Comprimento do contexto | Melhor para |
| --- | --- | --- | --- |
| **Qwen3-235B-A22B** | MdE | 128K | Tarefas de pesquisa, fluxos de trabalho de agentes, cadeias de raciocínio longas |
| **Qwen3-30B-A3B** | MdE | 128K | Raciocínio equilibrado com menor custo de inferência |
| **Qwen3-32B** | Dense | 128K | Implantações de uso geral de alto nível |
| **Qwen3-14B** | Dense | 128K | Aplicativos de médio porte que precisam de raciocínio sólido |
| **Qwen3-8B** | Dense | 128K | Tarefas de raciocínio leves |
| **Qwen3-4B** | Dense | 32K | Aplicativos menores, inferência mais rápida |
| **Qwen3-1.7B** | Dense | 32K | Casos de uso móveis e incorporados |
| **Qwen3-0.6B** | Dense | 32K | Configurações muito leves ou restritas |

Se você estivertrabalhando em tarefas que exijam raciocínio mais profundo, uso de ferramentas de agente ou manuseio de contextos longos, o Qwen3-235B-A22B lhe dará a maior flexibilidade.

Para os casos em que você deseja manter a inferência mais rápida e barata e, ao mesmo tempo, lidar com tarefas moderadamente complexas, o Qwen3-30B-A3B é uma boa opção.

Os modelos densos oferecem implementações mais simples e latência previsível, o que os torna mais adequados para aplicativos de menor escala.

## Como o Qwen3 foi desenvolvido

Os modelos Qwen3 foram criados por meio de uma fase de pré-treinamento de três estágios, seguida por um pipeline de pós-treinamento de quatro estágios.

O pré-treinamento é quando o modelo aprende padrões gerais a partir de grandes quantidades de dados (linguagem, lógica, matemática, código) sem que você saiba exatamente o que fazer. O pós-treinamento é quando o modelo é ajustado para se comportar de maneiras específicas, como raciocinar com cuidado ou seguir instruções.

Vou examinar as duas partes em termos simples, sem me aprofundar em detalhes técnicos.

### Pré-treinamento

Em comparação com o Qwen2.5, o conjunto de dados de pré-treinamento do Qwen3 foi significativamente ampliado. Cerca de 36 trilhões de tokens foram usados, dobrando a quantidade da geração anterior. Os dados incluíam conteúdo da Web, texto extraído de documentos e exemplos sintéticos de matemática e código gerados pelos modelos Qwen2.5.

O processo de pré-treinamento seguiu três etapas:

- Estágio 1: As habilidades básicas de linguagem e conhecimento foram aprendidas usando mais de 30 trilhões de tokens, com um comprimento de contexto de 4K.
- Estágio 2: O conjunto de dados foi refinado para aumentar a participação de dados STEM, de codificação e de raciocínio, seguido por mais 5 trilhões de tokens.
- Estágio 3: Dados de contexto longo de alta qualidade foram usados para estender os modelos a janelas de contexto de 32K.

![qwen 3 estágios de pré-treinamento](https://media.datacamp.com/cms/ad_4nxfw2ijikdil662gk6si_4cxtrjq_i_wijdtmpshgihqtkly_ka8g1ybgsm9nk78q8wwo6zuiskat-kwf3bp23rrurbvfyto5sl78_ehlnst0sec9o-1vq3b6i994ukqbhxpierxug.png)

O resultado é que os modelos de base Qwen3 densos se igualam ou superam os modelos de base Qwen2.5 maiores, usando menos parâmetros, especialmente em tarefas STEM e de raciocínio.

### Pós-treinamento

O pipeline pós-treinamento do Qwen3 concentrou-se na integração de raciocínio profundo e recursos de resposta rápida em um único modelo. Primeiro, vamos dar uma olhada no diagrama abaixo e, em seguida, explicarei passo a passo:

![qwen 3 pipeline pós-treinamento](https://media.datacamp.com/cms/ad_4nxeqfwg1otcvbzpipjom9glnsh_dqoi9j-ocny2ucme1bbgsedl10s4qn9cyetudzy97zs25earodfv3s0ay4kfo7arbe2magilzmrt9v_hlk5gqo-6eh-b5j0-swgghuvedacid.png)

Pipeline pós-treinamento do Qwen 3. Fonte: [Qwen](https://qwenlm.github.io/blog/qwen3/)

Na parte superior (em laranja), você pode ver o caminho de desenvolvimento dos "Frontier Models" maiores, como o Qwen3-235B-A22B e o Qwen3-32B. Tudo começa com uma Longa [cadeia de raciocínio](https://www.datacamp.com/pt/tutorial/chain-of-thought-prompting) Cold Start (estágio 1), em que o modelo aprende a raciocinar passo a passo em tarefas mais difíceis.

Isso é seguido por Raciocínio [Aprendizado por reforço](https://www.datacamp.com/pt/tracks/reinforcement-learning) (RL) (estágio 2) para estimular melhores estratégias de solução de problemas. No estágio 3, chamado Thinking Mode Fusion, Qwen3 aprende a equilibrar o raciocínio lento e cuidadoso com respostas mais rápidas. Por fim, o estágio de RL geral do aprimora seu comportamento em uma ampla gama de tarefas, como o acompanhamento de instruções e casos de uso de agentes.

Abaixo disso (em azul claro), você verá o caminho para os "Modelos leves", como o Qwen3-30B-A3B e os modelos menores e densos. Esses modelos são treinados usando a destilação de forte a fraco [destilação](https://www.datacamp.com/pt/blog/distillation-llm)um processo em que o conhecimento dos modelos maiores é compactado em modelos menores e mais rápidos, sem perder muita capacidade de raciocínio.

Em termos simples: os modelos grandes foram treinados primeiro e, em seguida, os modelos leves foram destilados a partir deles. Dessa forma, toda a família Qwen3 compartilha um estilo de pensamento semelhante, mesmo em modelos de tamanhos muito diferentes.

## Benchmarks do Qwen 3

Os modelos Qwen3 foram avaliados em uma série de benchmarks de raciocínio, codificação e conhecimento geral. Os resultados mostram que o Qwen3-235B-A22B lidera a linha de produtos na maioria das tarefas, mas os modelos menores Qwen3-30B-A3B e Qwen3-4B também oferecem bom desempenho.

### Qwen3-235B-A22B e Qwen3-32B

Na maioria dos benchmarks, o Qwen3-235B-A22B está entre os modelos de melhor desempenho, embora nem sempre seja o líder.

![](https://media.datacamp.com/cms/ad_4nxfrf7i6xad_3cttg_val9ypgpr_vujsog6mnxif279hqkxdnnmguefgladtvmjfbxpv-2i1eyi5f-vmmkhwfx23nskf6ccxwxbcaeddt9jz3llhzm06lss0nd2mbfeshyszorkl.png)

Fonte: [Qwen](https://qwenlm.github.io/blog/qwen3/)

Vamos explorar rapidamente os resultados acima:

- ArenaHard (raciocínio geral): [Gemini 2.5 Pro](https://www.datacamp.com/pt/blog/gemini-2-5-pro) lidera com 96,4. O Qwen3-235B está logo atrás, com 95,6, à frente do o1 e do DeepSeek-R1.
- AIME'24 / AIME'25 (matemática): Pontuações 85,7 e 81,4. O Gemini 2.5 Pro está novamente em uma posição superior, mas o Qwen3-235B ainda supera o DeepSeek-R1, o Grok 3 e o o3-mini.
- LiveCodeBench (geração de código): 70,7 para o modelo 235B - melhor do que a maioria dos modelos, exceto o Gemini.
- CodeForces Elo (programação competitiva): 2056, maior do que todos os outros modelos listados, incluindo o DeepSeek-R1 e o Gemini 2.5 Pro.
- LiveBench (tarefas gerais do mundo real): 77.1, novamente perdendo apenas para o Gemini 2.5 Pro.
- MultiIF (raciocínio multilíngue): O Qwen3-32B menor tem uma pontuação melhor aqui (73,0), mas ainda está atrás do Gemini (77,8).

### Qwen3-30B-A3B e Qwen3-4B

O Qwen3-30B-A3B (o modelo MoE menor) tem um bom desempenho em quase todos os benchmarks, consistentemente igualando ou superando modelos densos de tamanho semelhante.

- ArenaHard: 91,0 acima do QwQ-32B (89,5), DeepSeek-V3 (85,5) e GPT-4o (85,3).
- AIME'24 / AIME'25: 80,4 - um pouco à frente do QwQ-32B, mas quilômetros à frente dos outros modelos.
- CodeForces Elo: 1974 - apenas sob QwQ-32B (1982).
- GPQA (controle de qualidade em nível de pós-graduação): 65,8 - praticamente empatado com o QwQ-32B.
- MultiIF: 72,2 - superior ao QwQ-32B (68,3).

![](https://media.datacamp.com/cms/ad_4nxdahqtoykkjjfhq4efa4peyucwrlhif4eh8mnbbjjor3a1lli4zzdyrgelr3nagouvbyjffleu9hmsbf8txxnfopvwrzdvunpy2fncc8fawujylwygby31ap7mx0uw9wunvzsoxqa.png)

Fonte: [Qwen](https://qwenlm.github.io/blog/qwen3/)

O Qwen3-4B apresenta um desempenho sólido para seu tamanho:

- ArenaHard: 76.6
- AIME'24 / AIME'25: 73,8 e 65,6 - claramente mais fortes do que os modelos Qwen2.5 anteriores e muito maiores e modelos como o Gemma-27B-IT.
- CodeForces Elo: 1671 - não é competitivo com os modelos maiores, mas está no mesmo nível de sua categoria de peso.
- MultiIF: 66,3-Respeitável para um modelo denso de 4B, e notavelmente à frente de muitas linhas de base de tamanho semelhante.

## Como acessar o Qwen3

Os modelos Qwen3 estão disponíveis publicamente e podem ser usados no aplicativo de bate-papo, via API, baixados para implantação local ou integrados a configurações personalizadas.

### Interface de bate-papo

Você pode experimentar o Qwen3 diretamente em[chat.qwen.ai](https://chat.qwen.ai/).

Você só poderá acessar três modelos da família Qwen 3 no aplicativo de bate-papo: Qwen3-235B, Qwen3-30B e Qwen3-32B:

![qwen 3 modelos disponíveis no aplicativo de bate-papo](https://media.datacamp.com/cms/ad_4nxfbbfml7g_jzreviez_67i51v2lxor5bblbj2e9xcfk6yigv9f9pkkrkkwycpb1vjsjkvjv89ejkcx7krbggke0kt0rnerxd6mxaxywptjtxztj8l2cvzjvvdb7cb16xgelfwllow.png)

### Acesso à API do Qwen 3

O Qwen3 trabalha com formatos de API compatíveis com OpenAI por meio de provedores como ModelScope ou DashScope. Ferramentas como [vLLM](https://www.datacamp.com/pt/tutorial/vllm) e SGLang oferecem um serviço eficiente para implantação local ou auto-hospedada. O blog oficial do [O blog oficial do Qwen 3 tem mais detalhes sobre isso](https://qwenlm.github.io/blog/qwen3/#develop-with-qwen3).

### Pesos abertos

Todos os modelos Qwen3, tanto MoE quanto densos, são liberados sob a licença Apache 2.0. Eles estão disponíveis em:

- [Hugging Face](https://huggingface.co/collections/Qwen/qwen3-67dd247413f0e2e4f653967f)
- [ModelScope](https://modelscope.cn/collections/Qwen3-9743180bdc6b48)
- [Kaggle](https://www.kaggle.com/models/qwen-lm/qwen-3)

### Implementação local

Você também pode executar o Qwen3 localmente usando:

- Ollama
- LM Studio
- llama.cpp
- KTransformers

## Conclusão

O Qwen3 é uma das mais completas suítes de modelos de peso aberto lançadas até o momento.

O principal modelo 235B MoE tem um bom desempenho em tarefas de raciocínio, matemática e codificação, enquanto as versões 30B e 4B oferecem alternativas práticas para implantações em menor escala ou com orçamento limitado. A capacidade de ajustar o orçamento de raciocínio do modelo adiciona uma camada extra de flexibilidade para usuários regulares.

No momento, o Qwen3 é uma versão completa que abrange uma ampla gama de casos de uso e está pronto para ser usado em configurações de pesquisa e produção.

## Perguntas frequentes

### Posso usar o Qwen3 em produtos comerciais?

Sim. A licença Apache 2.0 permite o uso comercial, a modificação e a distribuição com atribuição.

### Posso fazer o ajuste fino dos modelos do Qwen3?

### O Qwen3 oferece suporte à chamada de funções ou ao uso de ferramentas?

### O Qwen3 oferece suporte multilíngue imediatamente?

Temas

Aprenda IA com estes cursos!

Programa

### AI Fundamentals

10hrs hr Discover the fundamentals of AI, dive into models like ChatGPT, and decode generative AI secrets to navigate the dynamic AI landscape.[Ver Detalhes](https://www.datacamp.com/pt/tracks/ai-fundamentals)[Iniciar curso](https://www.datacamp.com/pt/users/sign_up?redirect=%2Ftracks%2Fai-fundamentals%2Fcontinue)

Programa

### Llama Fundamentals

4hrs hr Experiment with Llama 3 to run inference on pre-trained models, fine-tune them on custom datasets, and optimize performance.[Ver Detalhes](https://www.datacamp.com/pt/tracks/llama-fundamentals)[Iniciar curso](https://www.datacamp.com/pt/users/sign_up?redirect=%2Ftracks%2Fllama-fundamentals%2Fcontinue)

Programa

### EU AI Act Fundamentals

8 hours hr Master the EU AI Act and AI fundamentals. Learn to navigate regulations and foster trust with Responsible AI.[Ver Detalhes](https://www.datacamp.com/pt/tracks/eu-ai-act-fundamentals)[Iniciar curso](https://www.datacamp.com/pt/users/sign_up?redirect=%2Ftracks%2Feu-ai-act-fundamentals%2Fcontinue)