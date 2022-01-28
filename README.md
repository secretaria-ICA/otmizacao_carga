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
Cada carga ao longo do tempo registra suas informações de execução em banco de dados a partir de solução de coleta, tratamento e armazenamento dos logs de execução. Com isso é possível levatar informações históricas que serão usadas como atributos para o algorítimo, que são tempo de carga e gasto computacional de cada banco de dados ingerido no Datalake.


### 2. Modelagem

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Proin pulvinar nisl vestibulum tortor fringilla, eget imperdiet neque condimentum. Proin vitae augue in nulla vehicula porttitor sit amet quis sapien. Nam rutrum mollis ligula, et semper justo maximus accumsan. Integer scelerisque egestas arcu, ac laoreet odio aliquet at. Sed sed bibendum dolor. Vestibulum commodo sodales erat, ut placerat nulla vulputate eu. In hac habitasse platea dictumst. Cras interdum bibendum sapien a vehicula.

Proin feugiat nulla sem. Phasellus consequat tellus a ex aliquet, quis convallis turpis blandit. Quisque auctor condimentum justo vitae pulvinar. Donec in dictum purus. Vivamus vitae aliquam ligula, at suscipit ipsum. Quisque in dolor auctor tortor facilisis maximus. Donec dapibus leo sed tincidunt aliquam.

### 3. Resultados

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Proin pulvinar nisl vestibulum tortor fringilla, eget imperdiet neque condimentum. Proin vitae augue in nulla vehicula porttitor sit amet quis sapien. Nam rutrum mollis ligula, et semper justo maximus accumsan. Integer scelerisque egestas arcu, ac laoreet odio aliquet at. Sed sed bibendum dolor. Vestibulum commodo sodales erat, ut placerat nulla vulputate eu. In hac habitasse platea dictumst. Cras interdum bibendum sapien a vehicula.

Proin feugiat nulla sem. Phasellus consequat tellus a ex aliquet, quis convallis turpis blandit. Quisque auctor condimentum justo vitae pulvinar. Donec in dictum purus. Vivamus vitae aliquam ligula, at suscipit ipsum. Quisque in dolor auctor tortor facilisis maximus. Donec dapibus leo sed tincidunt aliquam.

### 4. Conclusões

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Proin pulvinar nisl vestibulum tortor fringilla, eget imperdiet neque condimentum. Proin vitae augue in nulla vehicula porttitor sit amet quis sapien. Nam rutrum mollis ligula, et semper justo maximus accumsan. Integer scelerisque egestas arcu, ac laoreet odio aliquet at. Sed sed bibendum dolor. Vestibulum commodo sodales erat, ut placerat nulla vulputate eu. In hac habitasse platea dictumst. Cras interdum bibendum sapien a vehicula.

Proin feugiat nulla sem. Phasellus consequat tellus a ex aliquet, quis convallis turpis blandit. Quisque auctor condimentum justo vitae pulvinar. Donec in dictum purus. Vivamus vitae aliquam ligula, at suscipit ipsum. Quisque in dolor auctor tortor facilisis maximus. Donec dapibus leo sed tincidunt aliquam.


Matrícula: 192110171

Pontifícia Universidade Católica do Rio de Janeiro

Curso de Pós Graduação *Business Intelligence Master*
