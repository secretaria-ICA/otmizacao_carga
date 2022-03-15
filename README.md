# tcc_bimaster
Trabalho de Conclusão de Curso de Pós Graduação PUC-Rio - BI Master

#### Aluno: [André Luís Maravilha Franco da Silva](https://github.com/AndreLuisMaravilha)
#### Orientador: [Felipe Borges](https://github.com/FelipeBorgesC)

Trabalho apresentado ao curso [BI MASTER](https://ica.puc-rio.ai/bi-master) como pré-requisito para conclusão de curso e obtenção de crédito na disciplina "Projetos de Sistemas Inteligentes de Apoio à Decisão".

- [Link para o código](https://github.com/link_do_repositorio).
- [Link para a monografia](https://link_da_monografia.com). 

### Resumo

Com o constante crescimento das bases de dados nas mais diversas tecnologias e formatos a formação de Datalakes bem como a manutenção do constante fluxo de dados para os mesmos vem se tornando um desafio cada vez maior. 
Neste contexto os processos de ingestão de dados para um Datalake vem se tornando cada vez mais complexos, devendo ao mesmo tempo que mantém os dados e atualizados, estruturados e confiáves, também atender a diversas restrições de ambientes onde os dados são gerados e transacionados constantemente.
Para isso restrições como janelas para cargas de dados, limites computacionais das fontes dos dados, processos rotineiros como backup e manutenção das bases de origem, recursos computacionais para movimetação e uso dos ativos de rede são fatores que devem ser respeitados e combinados de forma que as cargas sejam executadas de forma eficaz e confiável. O seguinte trabalho propõe a utilização de Algorítimos Genéticos para elaboração de melhor agendamento possível de cada carga de dados realizada através de processos Bacth para dentro do Datalake, tomando como base a utilização de recursos e tempo de carga provenientes de dados históricos e tendo como objetivo final a minimização da janela de carga respesitada todas as restrições do processo. 


With the constant growth of databases in the most diverse technologies and formats, the creation of Datalakes as well as the maintenance of the constant flow of data for them has become an increasing challenge.
In this context, the data ingestion processes for a Datalake are becoming increasingly complex, and while keeping the data updated, structured and reliable, it must also meet the various restrictions of environments where data is constantly generated and transacted.
For this, restrictions such as windows for data loads, computational limits of data sources, routine processes such as backup and maintenance of source bases, computational resources for movement and use of network assets are factors that must be respected and combined so that the loads are carried out efficiently and reliably. The following work proposes the use of Genetic Algorithms for the elaboration of the best possible scheduling of each data load carried out through Bacth processes into Datalake, based on the use of resources and load time from historical data and having as a final objective the minimization of the load window respected all process constraints.



### 1. Introdução

Atualmente o ambiente produtivo onde este trabalho se propõe otimizar realiza em torno de 50 cargas diárias, totalizando a movimentação de mais de 40 Terabaytes de dados ao final delas. Estas cargas são agendadas levando em consideração os diversos fatores citados (janela, retrições, etc...), porém a sua ordenação de execução e ínicio são agendados mediante cálculos básicos e percepção dos Analistas que atuam no processo.
Cada carga ao longo do tempo, registra suas informações de execução em banco de dados a partir de solução de coleta, tratamento e armazenamento dos logs. Com isso é possível levatar informações históricas que serão usadas como atributos para o algorítimo, que são tempo de carga e gasto computacional de cada banco de dados ingerido no Datalake.


### 2. Modelagem

Devido às restriões da ferramenta utilizada a modelgaem foi realizada para o trabalho com 180 variáveis conforme disposto a seguir:

Foram eleitos para o estudo os nove banco de dados que mais influenciam as cargas no ambiente, de acordo com a mediana do gasto computacional e a mediana do tempo de carga de cada um. 
A janela de carga foi disposta com em intervalos de 5 minutos para início de cada carga, intervalos estes, que posteriomente recebem pesos de execução afim de formar a função objetivo. 
Desta forma a matriz de correlação é formada pelo banco de dados e horário de início de cada carga, sendo o valor da variável (célula) o gasto computacional para execução. 
A seguir será descrito cada intervalo de céclulas da planilha com sua respectiva função na modelagem. 

#### 2.1. Cromossomos
Posição: B2:U10

O intervalo possui valores de conteúdo binário que é usado como referência para composição da matriz de carga e é usado como entrada para o Solver como objeto de valores variáveis.
O conjunto de variáveis deste intervalo respresenta um indivíduo dentro do esquema de modelagem. 

#### 2.2. Dados Base
Posição: A13:C22

São os dados de valores fixos obtidos através de dados históricos das cargas que representam a mediana do tempo de carga de cada banco e a média de gasto computacional das mesmas.

#### 2.3. Blocos contiguos
Posição: E13:E22

Respresenta a quantidade de blocos contiguos de intervalos de tempo necessária para cada carga, levando em consideração blocos de 15 minutos neste estudo. 
Esta quantidade será utilizada como base para uma das rerições do algorítimo, pois cada carga uma vez iniciada deve ser finalizada, não havendo lapsos de tempo ou pausas durante o processo. 

#### 2.4 Peso de Hora
Posição: A27:U27

Indica o peso que cada horario possui. Os pesos aumentam linearmente de acordo com os intervalos de tempo e serão utilizados para a formação da função objetivo. 
Quanto maior o horário do bloco de tempo maior seu peso dentro da função objetivo.

#### 2.5 Limite de Fila
Posição: A25

Tamanho máximo da fila de recursos computacionais (cores de processamento do cluster) utilizada pelo processo de carga.
Valor fixo, será utilizado em uma das funções de retrição para o algorítimo. 

#### 2.6 Função Objetivo
Posição: L41

O Valor representa a função objetivo do algorítimo que será subetida ao processo de otimização - MINIMIZAÇÃO.
É formada pela resultate do somatório da coluna "Peso de horário final", desta forma quanto mais tarde for o término da carga de um banco específico, respeitada todas as restrições, maior será seu peso no aumento de valor da função objetivo.
Como o objetivo é de minimização a busca será sempre pelo menor peso/horário para término de cada processo de carga no algorítimo genético.

## Restrições: 

As restrições foram modeladas de forma a permitir a menor entrada possível no Solver, direcionando o processamento de fórmulas para camada anterior. 
Para melhor representação das fórmulas utilizadas na restrição será usada a seguinte simbologia: \

"nl": Representa o número de linha com incremento crescente de uma unidade no intervalo.\
"lc": Representa a letra da coluna com incremento crescente de uma unidade no intervalo.\


#### 2.7 Restrição de obrigatoriedade de carga
Posição: V2:V11

Tem como objetivo garantir que todos os banco de dados sejam carregados. Evitando que o algoritimo gere algum indivíduo válido que contenha uma linha de cromossomo inteira com valores iguais a zero, ou seja, excluindo a carga de um determinado banco de dados. 
Possui dois blocos de células dispostos da seguinte maneira: \

  Bloco1: V2:V10\
  Fórmula: "=OU(Bnl:Unl)"\
  Descrição: Verifica a existencia de pelo menos um valor VERDADEIRO na linha\ 
  
  Bloco2: V11\
  Fórmula: "=SE(E(V2:V10);1;0)"\
  Descrição: Verifica se todas as linhas do bloco1 possuem valor VERDADEIRO\
 
#### 2.8 Restrição Total de limite de fila
Posição: B38:V38

Verifica se o somatório de recursos computacionais em um determinado bloco de tempo é menor ou igual ao limte computacional da fila definido em B25
Possui dois blocos de células dispostos da seguinte maneira:

  Bloco1: B38:U38\
  Fórmula: "=SE(SOMA(lc29:lc37)<=$B$25;1;0)"\
  Descrição: Verifica se o somatório dos recursos computacionais de cada banco de dados se encaixa na fila naquele instante de tempo. \
  
  Bloco2: V38\
  Fórmula: "=SE(E(Bnl:Unl);1;0)"\
  Descrição: Verifica se todas as linhas do bloco1 possuem valor VERDADEIRO (1)\
  
#### 2.9 Restrição SOMA Cromossomo
Posição: W29:V38

Impede que um indivíduo tenha quantidade de cromossos nulos diferentes dos valores indicados para cada linha. Os valores não nulos devem ser iguais ao tempo total de carga dividido em blocos de 15 minutos, que por sua vez é igual a coluna de referêcia "Blocos contíguos" (E13)    
Possui três blocos de células dispostos da seguinte maneira: 

  Bloco1: W29:W37.\
  Fórmula: "=SE(SOMA(Bnl:Unl)=B14* E14;1;0)"\
  Descrição: Verifica se o somatório do valores da linha é igual a multiplicação da quantidade de recursos computacionais definido em B13 e a quantidade de blocos definida em     E13.
  
  Bloco2: V43:V51\
  Fórmula: "=SE(OU(Bnl:Unl);1;0)" onde nl -> 43..51\
  Descrição: Verifica se existe pelo menos um valor testado para cada linha do blovo com com resultado VERDADEIRO. Isso indica que na referida linha existe um cadeia de           cromossomos que respeita a restrição bloco sem valor nulo.
  
  Bloco3:V52\
  Fórmula: "=SE(E(V43:V51);1;0)"\
  Descrição: Verifica se todas as linhas do bloco2 possuem valor VERDADEIRO (1)
  
  
#### 2.10 Restricao Blocos Contíguos

Esta restrição foi implementada no modelo para garantir que toda carga ao ser iniciada seja finalizada sem interrupções, em outras palavavras para restringir indivíduos que possuam cromossomos de valor zero após ter um cromossomo de valor diferente de zero na linha da matriz principal.
Possui dois blocos de células dispostos da seguinte maneira: 

  Bloco1: B43:U51 onde nl -> 43..51\
  Fórmula: "=SE(SOMA(DESLOC(Y;;;;$X))=$Z*$X;1;0)"\
  Descrição: A fórmula acima verifica se para cada célula, o somatório de seu valor com os valores de X células deslocadas a para direita é igual ao pruduto da quantidade de       recursos gastos por X, onde X é o valor de blocos de tempo calculado para cada banco baseado em intervalos de 15 minutos, Y a celula testada e Z o valor do recurso               computacional gasto pelo banco testado dentro do intervalo (B14:B22). 
  
  Ex: "=SE(SOMA(DESLOC(B29;;;;$E14))=$B14*$E14;1;0)"
  
  Bloco2: W38\
  Fórmula: "=SE(E(W29:W37);1;0)"\
  Descrição: Verifica se todas as linhas do bloco1 possuem valor VERDADEIRO (1)\



### 3. Resultados

O modelo foi deste trabalho foi processado utilizando a ferramenta "Solver" no Microsof Excel e o método "Evolutionary".

O método utilizado é baseado na teoria da evolução das espécies, onde os indivíduos, mais fortes de uma população geram os indivíduos das popolações subsequentes, em outras palavaras, os valores mais promissores de um conjunto para alcançar o objetivo final são utilizados para gerar os valores do conjunto subsequente. 

Cada população possui seu conjunto de indivíduos válidos e mais promissores de acordo com as restrições aplicadas da modelagem.

Devido a geração e validação dos indivíduos de cada população ser realizada de forma individual, este método é mais lento que outros métodos do "Solver" porém possui mais eficácia na convergência para valores ótimos globais e menor taxa de parada e valores ótimos locais.

#### 3.1 Configuração do Solver

- Células Variáveis: $B$2:$U$10

Cromossomos que compões cada indivíduo gerado da população.

- Restrições: 

A primeira restrição define que cada cromossomo pode apenas assumir valores binários, que por sua vez referenciam a matriz de consumo de recursos por tempo: 

  - $B$2:$U$10 = binário

As demais restrições, conforme citado anteriormente, tiveram complexidade das funções modeladas através de fórmulas e matrizes no Excel, de forma que ficasse a cargo do Solver um teste lógico simples de Verdadeiro ou Falso para cada restrição.

  - $V$11 = 1 -> Restrição de obrigatoriedade de carga
  - $V$38 = 1 -> Restrição Total de limite de fi0,000001
  
  la
  - $V$52 = 1 -> Restricao Blocos Contíguos
  - $W$38 = 1 -> Restrição SOMA Cromossomo

![image](https://user-images.githubusercontent.com/66565707/157300694-0c64e5b9-729a-46e4-9c27-811de6ec6a0e.png)


#### 3.2 Parâmetros para o método Evolutionary: 

- Convergência:\
  Como o objetivo deste trabalho é encontrar o mais próximo possível do valor ótimo foi definido um valor baixo para este parâmetro. Com isso o Solver utiliza uma diferença máxima bem pequena para os melhores membros da população, fazendo com que mais indivíduos sejam testados. O tempo de execução aumementa mas a solução atinge valores mais próximo do ótimo global. 
  
  Valor utilizado: 0,000001
    
- Taxa de Mutação:\
  Esta popriedade determina a frequência relativa de mutação dos indivíduos, é determinada por valores entre 0 e 1, como o objetivo é chegar o mais próximo possível do ótimo     global foi atribuído um valor mais próximo de 1, mesmo que isso aumente o tempo total de execução.
  
  Valor utilizado: 0,75
    
- Tamanho da População:\
  Define o número de pontos (variáveis) que compõe uma solução candidata, como existe a limitação de 200 variáveis para o Solver, foi utilizado para este trabalho o valor 180.
  
  Valor utilizado: 180  
    
- Tempo Máximo sem aperfeiçoamento:\
  Como o objetivo deste trabalho é encontrar o mais próximo possível do valor ótimo foi definido um valor baixo para este parâmetro. Com isso o Solver utiliza uma diferença máxima bem pequena para os melhores membros da população, fazendo com que mais indivíduos sejam testados. O tempo de execução aumementa mas a solução atinge valores mais próximo do ótimo global. 
  
  Valor utilizado: 600
  
  
 #### 3.3 Cenários e Resultados
 
 - Cenário 1 

O primieito cenário foi iniciado com um indivíduo válido da população e com as configurações citadas acima. 

Valor da Função Objetivo: 545\
Horário de Finalização de todas as cargas: 04h30

Tempo de otimização: 55 minutos\
Valor da Função Objetivo: 410\
Horário de Finalização de todas as cargas: 03h45

Após o processo de otimização foi realizada uma segunda rodada do algoritimo partindo do resultado ótimo da primeira rodada do cenário. Os seguintes resultados foram alcançados:

Tempo de otimização: 32 minutos\
Valor da Função Objetivo: 405\
Horário de Finalização de todas as cargas: Não houve alteração.


 - Cenário 2

Como o tempo de máquina foi muito alto na primeira parametriação do algorítimo, no cenário foi realizada configuração do Solver de forma a tentar diminuir o tempo total, e entao verificar o quanto isso interfere no resultado da otimização em relação ao primeiro cenário.
O mesmo indivíduo de entrada foi utilizado para o processo.
A seguinte configuração foi realizada: 

- aaa
- bbb

Os seguintes resultados foram alcançados: 

- ccc
- ddd
-


### 4. Conclusões


Matrícula: 192110171

Pontifícia Universidade Católica do Rio de Janeiro

Curso de Pós Graduação *Business Intelligence Master*
