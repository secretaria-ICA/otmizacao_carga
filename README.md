# Otimização do agendamento de cargas de grandes volumes em ambientes de BIGDATA
Trabalho de Conclusão de Curso de Pós Graduação PUC-Rio - BI Master

#### Aluno: [André Luís Maravilha Franco da Silva](https://github.com/AndreLuisMaravilha)
#### Orientador: [Felipe Borges](https://github.com/FelipeBorgesC)

Trabalho apresentado ao curso [BI MASTER](https://ica.puc-rio.ai/bi-master) como pré-requisito para conclusão de curso e obtenção de crédito na disciplina "Projetos de Sistemas Inteligentes de Apoio à Decisão".

- [Link para o código](https://github.com/AndreLuisMaravilha/tcc_bimaster).
- [Link para a monografia](https://github.com/AndreLuisMaravilha/tcc_bimaster). 

### Resumo

Com o constante crescimento das bases de dados nas mais diversas tecnologias e formatos a formação de Datalakes bem como a manutenção do constante fluxo de dados para os mesmos vem se tornando um desafio cada vez maior. 
Neste contexto os processos de ingestão de dados para um Datalake vêm se tornando cada vez mais complexos, devendo ao mesmo tempo que mantém os dados e atualizados, estruturados e confiáveis, também atender a diversas restrições de ambientes onde os dados são gerados e transacionados constantemente.
Para isso restrições como janelas para cargas de dados, limites computacionais das fontes dos dados, processos rotineiros como backup e manutenção das bases de origem, recursos computacionais para movimentação e uso dos ativos de rede são fatores que devem ser respeitados e combinados de forma que as cargas sejam executadas de forma eficaz e confiável. O seguinte trabalho propõe a utilização de Algoritmos Genéticos para elaboração de melhor agendamento possível de cada carga de dados realizada através de processos Batch para dentro do Datalake, tomando como base a utilização de recursos e tempo de carga provenientes de dados históricos e tendo como objetivo final a minimização da janela de carga respeitadas todas as restrições do processo. 


With the constant growth of databases in the most diverse technologies and formats, the creation of Datalakes as well as the maintenance of the constant flow of data for them has become an increasing challenge.
In this context, the data ingestion processes for a Datalake are becoming increasingly complex, and while keeping the data updated, structured and reliable, it must also meet the various restrictions of environments where data is constantly generated and transacted.
For this, restrictions such as windows for data loads, computational limits of data sources, routine processes such as backup and maintenance of source bases, computational resources for movement and use of network assets are factors that must be respected and combined so that the loads are carried out efficiently and reliably. The following work proposes the use of Genetic Algorithms for the elaboration of the best possible scheduling of each data load carried out through Batch processes into Datalake, based on the use of resources and load time from historical data and having as a final objective the minimization of the load window respected all process constraints.



### 1. Introdução

Atualmente o ambiente produtivo onde este trabalho se propõe otimizar realiza em torno de 50 cargas diárias, totalizando a movimentação de mais de 40 Terabytes de dados ao final delas. Estas cargas são agendadas levando em consideração os diversos fatores citados (janela, restrições, etc...), porém a sua ordenação de execução e início são agendados mediante cálculos básicos e percepção dos Analistas que atuam no processo.
Cada carga ao longo do tempo, registra suas informações de execução em banco de dados a partir de solução de coleta, tratamento e armazenamento dos logs. Com isso é possível levatar informações históricas que serão usadas como atributos para o algoritmo, que são tempo de carga e gasto computacional de cada banco de dados ingerido no Datalake.


### 2. Modelagem

Devido às restrições da ferramenta utilizada a modelagem foi realizada para o trabalho com 180 variáveis conforme disposto a seguir:

Foram eleitos para o estudo os nove bancos de dados que mais influenciam as cargas no ambiente, de acordo com a mediana do gasto computacional e a mediana do tempo de carga de cada um. 
A janela de carga foi disposta com em intervalos de 5 minutos para início de cada carga, intervalos estes, que posteriormente recebem pesos de execução afim de formar a função objetivo. 
Desta forma a matriz de correlação é formada pelo banco de dados e horário de início de cada carga, sendo o valor da variável (célula) o gasto computacional para execução. 
O arquivo em anexo detalha a codificação das fórmulas utilizadas na modelagem do problema, bem como posição e descrição de cada parte de modelo: 


[TCC_codificacao_formulas_modelagem.docx](https://github.com/AndreLuisMaravilha/tcc_bimaster/files/8466601/TCC_codificacao_formulas_modelagem.docx)


#### 2.1. Cromossomos
Posição: B2:U10

O intervalo possui valores de conteúdo binário que é usado como referência para composição da matriz de carga e é usado como entrada para o Solver como objeto de valores variáveis.
O conjunto de variáveis deste intervalo representa um indivíduo dentro do esquema de modelagem. 

#### 2.2. Dados Base
Posição: A13:C22

São os dados de valores fixos obtidos através de dados históricos das cargas que representam a mediana do tempo de carga de cada banco e a média de gasto computacional das mesmas.

#### 2.3. Blocos contíguos
Posição: E13:E22

Representa a quantidade de blocos contíguos de intervalos de tempo necessária para cada carga, levando em consideração blocos de 15 minutos neste estudo. 
Esta quantidade será utilizada como base para uma das restrições do algoritmo, pois cada carga uma vez iniciada deve ser finalizada, não havendo lapsos de tempo ou pausas durante o processo. 

#### 2.4 Peso de Hora
Posição: A27:U27

Indica o peso que cada horário possui. Os pesos aumentam linearmente de acordo com os intervalos de tempo e serão utilizados para a formação da função objetivo. 
Quanto maior o horário do bloco de tempo maior seu peso dentro da função objetivo.

#### 2.5 Limite de Fila
Posição: A25

Tamanho máximo da fila de recursos computacionais (cores de processamento do cluster) utilizada pelo processo de carga.
Valor fixo, será utilizado em uma das funções de restrição para o algoritmo. 

#### 2.6 Função Objetivo
Posição: L41

O Valor representa a função objetivo do algoritmo que será submetida ao processo de otimização - MINIMIZAÇÃO.
É formada pela resultante do somatório da coluna "Peso de horário final", sendo o peso um valor crescente e proporcional ao horário de término.
Desta forma quanto mais tarde for o término da carga de um banco específico, respeitada todas as restrições, maior será seu peso no aumento de valor da função objetivo.
Como o objetivo é de minimização a busca será sempre pelo menor peso/horário para término de cada processo de carga no algoritmo genético.
 

## Restrições: 

#### 2.7 Restrição de obrigatoriedade de carga
Posição: V2:V11

Tem como objetivo garantir que todos os bancos de dados sejam carregados. Evitando que o algoritmo gere algum indivíduo válido que contenha uma linha de cromossomo inteira com valores iguais a zero, ou seja, excluindo a carga de um determinado banco de dados. 
Possui dois blocos de células dispostos da seguinte maneira: \

 
#### 2.8 Restrição Total de limite de fila
Posição: B38:V38

Verifica se o somatório de recursos computacionais em um determinado bloco de tempo é menor ou igual ao limite computacional da fila definido em B25
Possui dois blocos de células dispostos da seguinte maneira:
  
#### 2.9 Restrição SOMA Cromossomo
Posição: W29:V38

Impede que um indivíduo tenha quantidade de cromossomos nulos diferentes dos valores indicados para cada linha. Os valores não nulos devem ser iguais ao tempo total de carga dividido em blocos de 15 minutos, que por sua vez é igual a coluna de referência "Blocos contíguos" (E13)    
Possui três blocos de células dispostos da seguinte maneira: 
  
#### 2.10 Restrição Blocos Contíguos

Esta restrição foi implementada no modelo para garantir que toda carga ao ser iniciada seja finalizada sem interrupções, em outras palavras para restringir indivíduos que possuam cromossomos de valor zero após ter um cromossomo de valor diferente de zero na linha da matriz principal.
Possui dois blocos de células dispostos da seguinte maneira: 

  
#### 2.11 Restrição de Janela de Carga
  
A restrição de janela indica qual horário máximo que as cargas devem estar todas finalizadas. 
Neste modelo as cargas podem iniciar a partir da 00:00 e finalizar até o limite de 04:30
 
### 3. Resultados

O modelo foi deste trabalho foi processado utilizando a ferramenta "Solver" no Microsof Excel e o método "Evolutionary".

O método utilizado é baseado na teoria da evolução das espécies, onde os indivíduos, mais fortes de uma população geram os indivíduos das populações subsequentes, em outras palavras, os valores mais promissores de um conjunto para alcançar o objetivo final são utilizados para gerar os valores do conjunto subsequente. 

Cada população possui seu conjunto de indivíduos válidos e mais promissores de acordo com as restrições aplicadas da modelagem.

Devido a geração e validação dos indivíduos de cada população ser realizada de forma individual, este método é mais lento que outros métodos do "Solver" porém possui mais eficácia na convergência para valores ótimos globais e menor taxa de parada e valores ótimos locais.

#### 3.1 Configuração do Solver

##### Células Variáveis: $B$2:$U$10

Cromossomos que compões cada indivíduo gerado da população.

##### Restrições: 

As restrições, conforme citado anteriormente, tiveram complexidade das funções modeladas através de fórmulas e matrizes no Excel, de forma que ficasse a cargo do Solver um teste lógico simples de Verdadeiro ou Falso para cada restrição.

  - $B$2:$U$10 = binário
  - $V$11 = 1 -> Restrição de obrigatoriedade de carga
  - $V$38 = 1 -> Restrição Total de limite de fila
  - $V$52 = 1 -> Restrição Blocos Contíguos
  - $W$38 = 1 -> Restrição SOMA Cromossomo

#### 3.2 Parâmetros para o método Evolutionary: 

Os seguintes parâmetros foram considerados para configuração do método de otimização.

##### Convergência:

Determina a diferença máxima para os melhores membros da população, valores menores fazem com que mais indivíduos sejam testados. Neste caso tempo de execução aumementa mas a solução atinge valores mais próximo do ótimo global. 
   
##### Taxa de Mutação:

Esta propriedade determina a frequência relativa de mutação dos indivíduos, é determinada por valores entre 0 e 1.
      
##### Tamanho da População:

Define o número de pontos (variáveis) que compõe uma solução candidata, como existe a limitação de 200 variáveis para o Solver, foi utilizado para este trabalho o valor 180.
      
##### Tempo Máximo sem aperfeiçoamento:

Este parâmetro indica o tempo máximo em segundos sem melhoria significativa nos indivíduos da população.
    
#### 3.3 Cenários e Resultados
 
O arquivo a seguir apresenta os valores de cada parâmetro utilizado nos cenários testados: 
  
[TCC_codificacao_parametros_evolutionary.docx](https://github.com/AndreLuisMaravilha/tcc_bimaster/files/8524814/TCC_codificacao_parametros_evolutionary.docx)
 
Alguns cenários foram testados utilizando o modelo, alterando basicamente os parâmetros do método utilizado. 
 
A descrição de cada cenário e seus respectivos resultados são demonstrados a seguir: 

#### Cenário 1 
  
No primeiro cenário testado o objetivo foi tentar encontrar o mais próximo possível do ótimo global, independente do tempo de iteração do algoritmo.\
Neste cenário foi utilizado um valor muito baixo para a taxa de convergência e uma taxa de mutação próxima de 1. Esta faixa de valores apesar de aumentar teoricamente o tempo de execução, testam um maior número de indivíduos e possui maior potencial de alcançar o valor ótimo. 

Também visando alcançar valores mais próximos ao ótimo global o cenário 1 foi iniciado com um indivíduo válido da população, de forma que as próximas gerações tendam a possuir indivíduos candidatos com maior potencial.  

- Ponto de partida:[Cenario1_sem_Otimizacao.xlsx](https://github.com/AndreLuisMaravilha/tcc_bimaster/files/8351768/Cenario1_sem_Otimizacao.xlsx)

Valor da Função Objetivo: 550

Horário de Finalização de todas as cargas: 04:30

- Iteração 1:[Cenario1 - Otimizacao 1.xlsx](https://github.com/AndreLuisMaravilha/tcc_bimaster/files/8351769/Cenario1.-.Otimizacao.1.xlsx)\
Valor da Função Objetivo: 430\
Horário de Finalização de todas as cargas: 04:30.

- Iteração 2: [Cenario1 - Otimizacao 2.xlsx](https://github.com/AndreLuisMaravilha/tcc_bimaster/files/8351771/Cenario1.-.Otimizacao.2.xlsx)\
Valor da Função Objetivo: 400\
Horário de Finalização de todas as cargas: 03:45.

- Iteração 3:[Cenario1 - Otimizacao 3.xlsx](https://github.com/AndreLuisMaravilha/tcc_bimaster/files/8351774/Cenario1.-.Otimizacao.3.xlsx)

Valor da Função Objetivo: 375

Horário de Finalização de todas as cargas: 03:15.

- Iteração 4:[Cenario1 - Otimizacao 4.xlsx](https://github.com/AndreLuisMaravilha/tcc_bimaster/files/8351775/Cenario1.-.Otimizacao.4.xlsx)\
Valor da Função Objetivo: 355\
Horário de Finalização de todas as cargas: 03:15.

![image](https://user-images.githubusercontent.com/66565707/160136232-432c4b09-8e31-49ce-ab35-7f56ced0fc6c.png)

Após este ponto não aconteceu otimização do modelo.

#### Cenário 2

Como o tempo de máquina foi muito alto na primeira parametrização do algoritmo, no cenário 2 foi realizada configuração do Solver de forma a tentar diminuir o tempo total, e então verificar o quanto isso interfere no resultado da otimização em relação ao primeiro cenário.
O mesmo indivíduo de entrada foi utilizado para o processo.

Apenas o parâmetro de convergência foi alterado passando de iniciais 0,000001 para 0,01. O incremento como citado tem a intenção de tentar diminuir o tempo necessário de otimização. 

A primeira observação foi que o tempo de otimização não alterou de forma significante, tendo média de aproximadamente 20 minutos para cada iteração. 

Os seguintes resultados foram observados após as iterações: 

- Ponto de partida: [Cenario2_sem_Otimizacao.xlsx](https://github.com/AndreLuisMaravilha/tcc_bimaster/files/8351315/Cenario2_sem_Otimizacao.xlsx)\
Valor da Função Objetivo: 550\
Horário de Finalização de todas as cargas: 04:30

- Iteração 1: [Cenario2 - Otimizacao 1.xlsx](https://github.com/AndreLuisMaravilha/tcc_bimaster/files/8351323/Cenario2.-.Otimizacao.1.xlsx)\
Valor da Função Objetivo: 490\
Horário de Finalização de todas as cargas: 04:30.

- Iteração 2: [Cenario2 - Otimizacao 2.xlsx](https://github.com/AndreLuisMaravilha/tcc_bimaster/files/8351600/Cenario2.-.Otimizacao.2.xlsx)\
Valor da Função Objetivo: 460\
Horário de Finalização de todas as cargas: 04:00

- Iteração 3: [Cenario2 - Otimizacao 3.xlsx](https://github.com/AndreLuisMaravilha/tcc_bimaster/files/8351603/Cenario2.-.Otimizacao.3.xlsx)\
Valor da Função Objetivo: 435\
Horário de Finalização de todas as cargas: 03:45.

- Iteração 4: [Cenario2 - Otimizacao 4.xlsx](https://github.com/AndreLuisMaravilha/tcc_bimaster/files/8351605/Cenario2.-.Otimizacao.4.xlsx)\
Valor da Função Objetivo: 430\
Horário de Finalização de todas as cargas: 03:45.

- Iteração 5: [Cenario2 - Otimizacao 5.xlsx](https://github.com/AndreLuisMaravilha/tcc_bimaster/files/8351619/Cenario2.-.Otimizacao.5.xlsx)\
Valor da Função Objetivo: 425\
Horário de Finalização de todas as cargas: 03:45.

- Iteração 6: [Cenario2 - Otimizacao 6.xlsx](https://github.com/AndreLuisMaravilha/tcc_bimaster/files/8351621/Cenario2.-.Otimizacao.6.xlsx)\
Valor da Função Objetivo: 405\
Horário de Finalização de todas as cargas: 03:30.

- Iteração 7: [Cenario2 - Otimizacao 7.xlsx](https://github.com/AndreLuisMaravilha/tcc_bimaster/files/8351626/Cenario2.-.Otimizacao.7.xlsx)\
Valor da Função Objetivo: 400\
Horário de Finalização de todas as cargas: 03:30.

- Iteração 8: [Cenario2 - Otimizacao 8.xlsx](https://github.com/AndreLuisMaravilha/tcc_bimaster/files/8351628/Cenario2.-.Otimizacao.8.xlsx)\
Valor da Função Objetivo: 395\
Horário de Finalização de todas as cargas: 03:30.

- Iteração 9: [Cenario2 - Otimizacao 9.xlsx](https://github.com/AndreLuisMaravilha/tcc_bimaster/files/8351629/Cenario2.-.Otimizacao.9.xlsx)\
Valor da Função Objetivo: 385\
Horário de Finalização de todas as cargas: 03:30.

![image](https://user-images.githubusercontent.com/66565707/160002021-f3f675b9-0d51-456f-8098-e54ce519e440.png)

#### Cenário 3

No cenário 3 os mesmos parâmetros do Solver no cenário 1 foram utilizados, porém a otimização foi iniciada utilizando um indivíduo não válido da população.
Foram alterados por meio de mutação apenas 5 cromossomos dos 180 do modelo, isso fez com que duas restrições não fossem respeitadas (Restrição de janela de carga e Restrição de limite de fila).
Após a primeira iteração observou-se que não houve otimização do modelo e o processo foi interrompido após o limite configurado de 1000 segundos.

- Ponto de partida: [Cenario3_sem_Otimizacao.xlsx](https://github.com/AndreLuisMaravilha/tcc_bimaster/files/8524586/Cenario3_sem_Otimizacao.xlsx)\
Valor da Função Objetivo: 585\
Horário de Finalização de todas as cargas: 04:45

- Iteração 1: [Cenario3 - Iteracao 1.xlsx](https://github.com/AndreLuisMaravilha/tcc_bimaster/files/8524667/Cenario3.-.Iteracao.1.xlsx)\
Valor da Função Objetivo: 585\
Horário de Finalização de todas as cargas: 04:45.

#### Cenário 4

Como não houve otimização no cenário 3, o cenário 4 foi tentado realizando alterações significativas nos parâmetros do Solver de modo que mais indivíduos fossem testados e a diferença relativa entre os indivíduos válidos e não válidos fosse maior. Ao aumentar a taxa de mutação, espaçando os indivíduos espera-se encontrar indivíduos válidos de forma mais fácil a partir de um não válido de entrada. 
Porém ao rodar o modelo o mesmo comportamento do cenário 3 foi observado e não houve otimização do modelo dentro do limite de tempo configurado de 1000 segundos.

- Ponto de partida: [Cenario4_sem_Otimizacao.xlsx](https://github.com/AndreLuisMaravilha/tcc_bimaster/files/8524675/Cenario4_sem_Otimizacao.xlsx)\
Valor da Função Objetivo: 585\
Horário de Finalização de todas as cargas: 04:45

- Iteração 1:[Cenario4 - Iteracao 1 - sem_Otimizacao.xlsx](https://github.com/AndreLuisMaravilha/tcc_bimaster/files/8524679/Cenario4.-.Iteracao.1.-.sem_Otimizacao.xlsx)\
Valor da Função Objetivo: 585\
Horário de Finalização de todas as cargas: 04:45.


### 4. Conclusões

Conforme citado, no cenário 1 o objetivo foi alcançar o melhor valor para função objetivo independente do tempo e esforço para isso. Nota-se que neste caso ao configurar parâmetros mais extremos para o método Evolutionary o algoritmo se comportou bem, alcançando uma boa taxa de minimização com apenas uma iteração.
Mesmo o tempo não sendo um fator primordial neste cenário o limite de 1000 segundos foi aplicado, fazendo com que o ótimo global não fosse atingido com apenas uma iteração.

Ainda no cenário 1 nota-se que ao iniciar o método contendo um indivíduo apto mais forte, resultado da iteração anterior, a otimização atinge resultados ainda melhores, sem aumentar de forma considerável fatores de tempo e recurso computacional. 

No cenário 2 com uma taxa de convergência maior, o que espaçava mais os indivíduos, a otimização ocorreu de forma mais lenta, sendo necessário maior número de iterações para alcançar valores melhores. 

Nos cenários 3 e 4 a tentativa foi de iniciar o método utilizando indivíduos menos aptos da população, que não respeitasse uma ou mais restrições do modelo observando o quanto esse comportamento iria interferir no modelo. Neste caso observou-se que o modelo não alcançou nível de melhoria dentro do limite de tempo, além de ter utilizado maior recurso de processamento. 

Observando o comportamento do cenário 1 e principalmente dos cenários 3 e 4 conclui-se que a inicialização do modelo com indivíduos mais aptos na população interfere de forma significativa o desempenho e o resultado final do método de evolução escolhido. 

Utilizando um indivíduo menos apto, mesmo com uma taxa alta de mutação, não influenciou de forma satisfatória o resultado final. Confirmando a importância do ponto de partida ser a partir de indivíduos mais aptos da população. 

Este trabalho foi limitado em quantidade de variáveis devido a limitações do Excel, sendo vislumbrado como trabalho futuro implementação do modelo em Python com processamento em Spark, aumentando desta forma o escopo da otimização. 

---

Matrícula: 192110171

Pontifícia Universidade Católica do Rio de Janeiro

Curso de Pós Graduação *Business Intelligence Master*
