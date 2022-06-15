<!-- antes de enviar a versão final, solicitamos que todos os comentários, colocados para orientação ao aluno, sejam removidos do arquivo -->
# Identificação e Previsão de Abastecimento em Papeleira

#### Aluno: [Vinícius Souza Nascimento](https://github.com/link_do_github)
#### Orientador: [Manoela Kohler](https://github.com/link_do_github) e [Felipe Borges](https://github.com/FelipeBorgesC).

---

Trabalho apresentado ao curso [BI MASTER](https://ica.puc-rio.ai/bi-master) como pré-requisito para conclusão de curso e obtenção de crédito na disciplina "Projetos de Sistemas Inteligentes de Apoio à Decisão".

---

### Resumo

<!-- trocar o texto abaixo pelo resumo do trabalho, em português -->

O trabalho possui dois objetivos, o primeiro sendo identificar quando há abastecimento de papel em papeleiras de banheiro, por meio de uma base de dados da variação em porcentagem da quantidade de papel ao longo do tempo. O segundo objetivo é conseguir prever quando ocorrerá o próximo abastecimento, por meio do cruzamento de informações do nível do papel com a identificação de abastecimentos através do primeira etapa e uma segunda base de dados com a informação do fluxo de pessoas ao longo do tempo.

### Abstract <!-- Opcional! Caso não aplicável, remover esta seção -->

<!-- trocar o texto abaixo pelo resumo do trabalho, em inglês -->

The work has two objectives, the first being to identify when there is restock of paper towels, through a database of the variation in percentage of the amount of paper over time. The second objective is to be able to predict when the next restock will occur, by crossing information from the paper level with the identification of restocks through the first step and a second database with information on the flow of people over time.

### 1. Introdução

Antes de se montar o modelo é importante entender como os dados se comportam em ambas as bases de dados utilizadas.

A base de dados sobre o fluxo de pessoas possui um comportamento padrão ao longo do tempo. A contagem se inicia meia-noite e vai aumentando gradualmente ao longo do dia até que atinge o seu pico as 23:59 e a contagem é reiniciada para o próximo dia.

![alt text](https://github.com/Vinicius94SN/BIMaster/tree/main/Images/fluxo_pessoas.png?raw=true)


A base de dados sobre o percentual de papel dentro da papeleira também tem um comportamento padrão. A porcentagem de papel vai descendo ao longo do tempo até que há um um salto para 100% ou valor perto. A ocorrencia desse salto no valor é o comportamento especifico do abastecimento, porém há um problema de oscilação nos valores.

Como é possível ver na imagem acima, devico a sensibilidade do sensor com a constante manuseio da papeleira, a leitura do sensor pode oscilar de forma consideravel, criando comportamentos que podem ser confundidos com abastecimentos.

Sendo que o importante para o modelo é a identificação de uma mudança brusca no nível, a base de dados será  tratada substituindo os valores brutos do nível por suas medianas. A diferença pode ser notada na imagem abaixo, onde o comportamento do gráfico continua o mesmo porém com menos oscilações nos valores.

### 2. Modelagem
#### 2.1 Modelo de Classificação

#### 2.2 Modelo de Regressão


### 3. Resultados

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Proin pulvinar nisl vestibulum tortor fringilla, eget imperdiet neque condimentum. Proin vitae augue in nulla vehicula porttitor sit amet quis sapien. Nam rutrum mollis ligula, et semper justo maximus accumsan. Integer scelerisque egestas arcu, ac laoreet odio aliquet at. Sed sed bibendum dolor. Vestibulum commodo sodales erat, ut placerat nulla vulputate eu. In hac habitasse platea dictumst. Cras interdum bibendum sapien a vehicula.

Proin feugiat nulla sem. Phasellus consequat tellus a ex aliquet, quis convallis turpis blandit. Quisque auctor condimentum justo vitae pulvinar. Donec in dictum purus. Vivamus vitae aliquam ligula, at suscipit ipsum. Quisque in dolor auctor tortor facilisis maximus. Donec dapibus leo sed tincidunt aliquam.

### 4. Conclusões

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Proin pulvinar nisl vestibulum tortor fringilla, eget imperdiet neque condimentum. Proin vitae augue in nulla vehicula porttitor sit amet quis sapien. Nam rutrum mollis ligula, et semper justo maximus accumsan. Integer scelerisque egestas arcu, ac laoreet odio aliquet at. Sed sed bibendum dolor. Vestibulum commodo sodales erat, ut placerat nulla vulputate eu. In hac habitasse platea dictumst. Cras interdum bibendum sapien a vehicula.

Proin feugiat nulla sem. Phasellus consequat tellus a ex aliquet, quis convallis turpis blandit. Quisque auctor condimentum justo vitae pulvinar. Donec in dictum purus. Vivamus vitae aliquam ligula, at suscipit ipsum. Quisque in dolor auctor tortor facilisis maximus. Donec dapibus leo sed tincidunt aliquam.

---

Matrícula: 123.456.789

Pontifícia Universidade Católica do Rio de Janeiro

Curso de Pós Graduação *Business Intelligence Master*
