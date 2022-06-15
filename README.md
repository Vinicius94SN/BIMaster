<!-- antes de enviar a versão final, solicitamos que todos os comentários, colocados para orientação ao aluno, sejam removidos do arquivo -->
# Identificação e Previsão de Abastecimento em Papeleira

#### Aluno: [Vinícius Souza Nascimento](https://github.com/Vinicius94SN)
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

![fluxo_pessoas](https://raw.githubusercontent.com/Vinicius94SN/BIMaster/main/Images/fluxo_pessoas.png?)


A base de dados sobre o percentual de papel dentro da papeleira também tem um comportamento padrão. A porcentagem de papel vai descendo ao longo do tempo até que há um um salto para 100% ou valor perto. A ocorrencia desse salto no valor é o comportamento especifico do abastecimento, porém há um problema de oscilação nos valores.

![nivel](https://raw.githubusercontent.com/Vinicius94SN/BIMaster/main/Images/nivel_bruto.png?)

Como é possível ver na imagem acima, devico a sensibilidade do sensor com a constante manuseio da papeleira, a leitura do sensor pode oscilar de forma consideravel, criando comportamentos que podem ser confundidos com abastecimentos.

![mediana_nivel](https://raw.githubusercontent.com/Vinicius94SN/BIMaster/main/Images/mediana_nivel.png?)

Sendo que o importante para o modelo é a identificação de uma mudança brusca no nível, a base de dados será  tratada substituindo os valores brutos do nível por suas medianas. A diferença pode ser notada na imagem abaixo, onde o comportamento do gráfico continua o mesmo porém com menos oscilações nos valores.

### 2. Modelagem
#### 2.1 Modelo de Classificação

O modelo de classificação utilizará como informação de entrada o histórico do nível para o teste e treino e, para a saída, a informação do que a série de valores de nível representa. Como exemplo de como se pode classificar o comportamento do nível se tem: 

- Descida do nível
- valor constante 
- Oscilação de subida 
- Oscilação de descida 
- Abastecimento (Refil)

Já existe um script que, utilizando diversas condições para identificar grande parte dos cenários, fornece a informação se a sequencia de valores representa um abastecimento ou outro comportamento.

Em relação ao histórico do nível, será escolhido um ponto "n" na base de dados e seus 4 valores anteriores e posteriores, como representado na imagem abaixo:

![histórico_nivel](https://raw.githubusercontent.com/Vinicius94SN/BIMaster/main/Images/exemplo%20hist%C3%B3rico.png?)

Onde os valores de n+1 até n+4 são os valores anteriores e n-1 a n-4 os valores seguintes.

Como pode ocorrer problemas de conexão ou desligamento do equipamento, o intervalo de tempo entre duas leituras do nível pode ser muito maior que o normal de 15 minutos, chegando a horas de diferença. Por isso também será adicionado aos dados de entrada a informação de tempo médio entre cada leitura do nível e o tempo de leitura total do "n+4" até o "n-4".

A imagem abaixo representa o dataframe das informações de entrada e saída (A coluna: Status) do modelo

![inputs_e_outputs](https://raw.githubusercontent.com/Vinicius94SN/BIMaster/main/Images/exemplo%20input%20e%20output.png?)

Como não se tem dados suficientes para um grande número de exemplos de cada comportamento, serão gerados dados a partir dos já existentes. Aleatóriamente, dados já classificados irão ser escolhidos e valores pequenos (exemplo: de 1 a 5) serão adicionados ou subtraídos das colunas "n+4" até "n-4". Essa geração de dados não irá interferir com o treinamento do modelo pois o que o modelo precisa aprender é o comportamento do nível.

![comportamento](https://raw.githubusercontent.com/Vinicius94SN/BIMaster/main/Images/exemplo%20comportamento%20do%20hist%C3%B3rico.png?)

Na imagem acima foi utilizado como base para a geração de dados a linha 58. Nas linhas 59 a 61 foram somados os valores 1, 2 e 3 em todas as colunas com base nos valores da linha 58, enquanto que as linhas 62 a 64 foram subtraídos os valores 1, 2 e 3. É importante notar que apesar dos valores serem um pouco diferentes do original, o seu comportamento ao longo do tempo (da coluna "n+4" até "n-4") se mantém o mesmo. Da coluna "n+4" até a coluna "n+1" os valores foram diminuindo, o que condiz com o comportamento da papeleira, até que houve uma subida repentina no valor na coluna "n" que se manteve alto nas colunas seguintes.

Como o que está sendo analisado é o ponto na coluna "n", esse ponto seria considerado como o momento em que houve o abastecimento.

Após alguns testes do modelo se chegou na conclusão que mesmo com a geração de novos dados, não se obteve uma quantidade suficiente para o modelo aprender a classificar cada comportamento. Porém, como o objetivo é apenas identificar a ocorrencia do abastecimento, foi realizar alteração para que as informações de saída do modelo só classifique a sequencia de valores como dois comportamentos:

- Refil
- Not Refil

![modelos](https://raw.githubusercontent.com/Vinicius94SN/BIMaster/main/Images/cria%C3%A7%C3%A3o%20dos%20modelos.png?)

Com a alteração foi possível obter uma boa proporção entre cada comportamento nos dados de treino e teste

![modelos](https://raw.githubusercontent.com/Vinicius94SN/BIMaster/main/Images/propor%C3%A7%C3%A3o%20de%20dados.png?)

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
