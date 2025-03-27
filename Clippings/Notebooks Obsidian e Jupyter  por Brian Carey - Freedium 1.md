---
title: Notebooks Obsidian e Jupyter | por Brian Carey - Freedium
url: https://freedium.cfd/https://medium.com/@biscotty666/obsidian-and-jupyter-notebooks-5d90ab3eab4c
author: 
published: 
data: 2025-03-13T17:23:43-03:00
description: Personal Knowledge Management for Data Science
tags:
  - clippings
Atualizado: 2025-03-27  16.52
Criado: 2025-03-13  17.23
---


### Configurando o Obsidiano

O plugin da comunidade necessário para conseguir isso é chamado [de Execute Code](https://github.com/twibiral/obsidian-execute-code), escrito por Tim Wibiral, juntamente com o próprio Jupyter e uma biblioteca Python chamada "nbconvert". Este último converterá os blocos de anotações em markdown, e o plugin permite que você execute o código diretamente dentro de uma nota. Para começar, crie um ambiente virtual para o Obsidian usar. Se você não estiver usando ambientes virtuais, por favor comece agora! É simples, e você vai evitar problemas futuros. Da linha de comando, faça:

```bash
mkdir -p $HOME/.config/venvs && cd "$_"
python -m venv obsidian_venv
source obsidian_venv/bin/activate
pip install --upgrade pip
pip install jupyterlab nbconvert 
```

Você também pode instalar outras bibliotecas como Pandas e Matplotlib, bem como no ambiente virtual com pip install. O Jupyter lab e o nbconvert serão necessários para converter os cadernos em markdown. Neste ponto, você poderia lançar o laboratório de jupyter, mas não há necessidade. Após instalar pacotes, você pode sair do ambiente virtual com "desativar". Se você precisar instalar mais pacotes mais tarde, você pode digitar "source $HOME/.config/venvs/obsidian\_venvv/bin/activate" para reinserir no ambiente virtual e "pip install" outros pacotes.

Na Obsidian, instale o plugin Execute Code. Depois de instalar o plugin, você deve apontá-lo para a versão do Python que você deseja usar, neste caso o que fizemos no ambiente virtual acima. Nas configurações do plugin, nas configurações específicas do idioma, escolha Python na lista suspensa. Para *Python Path*, digite "/home/directory/.config/venvs/obsidian\_venv/bin/python".

Com isso feito, qualquer bloco de código com a palavra-chave "python", adicionada diretamente após os ticks de abertura do bloco, pode ser executado na nota. Em Visualização de leitura, um botão *Executar* será exibido por cada bloco de código, permitindo que você execute o código no bloco. Após a execução, haverá um botão *Limpar* para limpar a saída que você deseja remover da nota. O código também pode ser executado a partir do Edit View usando a palavra-chave "run-python" em vez de simplesmente "Wpython" (em inglês). O plugin oferece um comando para executar todo o código na nota, bem como um comando para visualizar e matar qualquer tempo de execução ativo.

### Processamento de um notebook

Jupyter Lab pode exportar um arquivo de ipynb diretamente para marcar! A partir da escrita, o VS Code só pode exportar para py, pdf ou html. No menu de arquivos do Jupyter Lab, selecione *Exportar* e escolha *Markdown*. Isso gerará um arquivo zipado contendo uma página de markdown, juntamente com quaisquer arquivos de imagem no bloco de anotações. O arquivo de marcação pode ser colocado onde você quiser no vault, e as imagens podem ser colocadas no diretório de anexos. O problema com essa abordagem. no entanto, é que todos os arquivos de imagem são nomeados "saída" algo, e assim, depois de exportar alguns cadernos, haverá conflitos de nome em seu cofre.

Usar a linha de comando evita esse problema e é, em qualquer caso, muito mais eficiente. Você precisará ativar seu ambiente virtual com o comando "fonte", conforme descrito acima. Em seguida, digite:

```bash
jupyter nbconvert --to Markdown your_notebook.ipynb
```

Isso gerará um arquivo de marcação que pode ser copiado para o seu cofre. Se houver imagens geradas pela saída, como gráficos e outros recursos visuais, elas serão colocadas em um novo diretório criado pelo comando acima. Se você copiar este diretório, com todos os arquivos de imagem no diretório vault que você usa para anexos, a nova nota irá encontrá-los. (É importante copiar o próprio diretório e não apenas os arquivos, já que a nova nota espera encontrá-los lá.)

Uma vez em Obsidian você pode processar seus Notebooks como você faria com qualquer outro arquivo, quebrando-os em pedaços de tamanho de mordida da bondade atômica. Eu confio fortemente em "Extrair este título" para este propósito, disponível com um clique com o botão direito em qualquer título / seção em uma nota. Isso substitui a seção por um link para uma nota recém-criada contendo o conteúdo da seção. Eu acho útil aplicar um modelo que carrega bibliotecas usadas com frequência, já que elas precisarão ser adicionadas na parte superior de todos os novos arquivos criados dessa maneira.

Ao converter seus blocos de anotações em notas, esteja ciente de que notas diferentes não compartilham o mesmo tempo de execução. Certifique-se de incluir todas as variáveis / cálculos necessárias para a parte do código que você extraiu no novo arquivo, pois isso não estará disponível no estado de outro arquivo. Além disso, todas as imagens que você vincular nas seções de marcação do bloco de anotações original precisarão ser copiadas manualmente para o cofre, pois apenas as imagens geradas a partir do código no arquivo serão exportadas.

