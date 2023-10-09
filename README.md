# <div align="center">O que aprendi nessa Sprint</div>

Aqui falarei um pouco sobre minha experiência ao longo da **Sprint 6**. Mas antes disso, confira minha apresentação para conhecer um pouco mais sobre mim [clicando aqui](https://github.com/t0nyn/Programa-de-Bolsas-Compasso/blob/main/README.md).

# <div align="center">Exercícios</div>

Foram realizados 3 exercícios nessa Sprint, sendo eles :

- **Exercício AWS S3**
  - print da criação do **Bucket** ![bucket s3](/Sprint-6/Exercicios/exercicio-S3/S3-bucket.jpg)
  - print do site hospedado ![site hospedado](/Sprint-6/Exercicios/exercicio-S3/S3-site.jpg)
- **Exercício AWS Athena**

  - Query para a consulta dos 3 nomes mais usados em cada década desde 1950

              SELECT nome, total, decada
              FROM (SELECT nome,
                          SUM(total) as total,
                          (ano - (ano%10)) AS decada,
                          ROW_NUMBER() OVER (PARTITION BY (ano - (ano%10)) ORDER BY SUM(total) DESC) AS rank_decada
                  FROM meubanco.pessoas
                  WHERE ano>=1950
                  GROUP BY nome, (ano - (ano%10))) AS rank_nomes_decada
              WHERE rank_decada<=3
              ORDER BY decada ASC, total DESC;

  - Resultado da Query ![resultado query](/Sprint-6/Exercicios/exercicio-Athena/resultadoquery.jpg)

- **Exercício AWS Lambda**
  - Caso de teste final aonde é mostrado o número de linhas do arquivo. Obs: esse print ficou meio borrado mas basta clicar nele que você será redirecionado para a página do print e lá a qualidade está melhor ![teste](/Sprint-6/Exercicios/exercicio-Lambda/testeFinal.jpg)

Para não poluir essa página com diversos prints, deixei alguns prints da execução dos exercícios nas seguintes pastas:

- [Exercício Athena](/Sprint-6/Evidencias/exercicio-Athena/)
- [Exercício Lambda](/Sprint-6/Evidencias/exercicio-Lambda/)

# <div align="center">Certificados</div>

Como são 13 certificados, decidi amostrá-los em outro local, para visualizar os certificados basta [clicar aqui](/Sprint-6/Certificados/README.md)

# <div align="center">Data Analytics</div>

Componentes da solução de avaliação de dados :

- Ingerir/Coletar : **etapa inicial, envolve a coleta de dados brutos de transações, logs e dispositivos**
- Armazenar : **armazenar os dados com segurança, de forma escalável e durável por meio de datastores estruturados, semiestruturados ou não estruturados**
- Processar/Analisar : **aqui ocorre o processamento dos dados, aplicando lógica de negócio para produzir conjuntos de dados significativos**
- Consumir/Visualizar : **produz, por meio de consultas ou utilizando ferramentas de Business Intelligence, avaliação rápida por meio da análise dos dados**

Desafios de gerenciamento de dados :

- Volume : **quantidade de dados que serão ingeridos/coletados. O problema em questão é a eficiência em coletar essas grandes quantidades de dados**
- Velocidade : **rapidez no processamento dos dados, hoje em dia a exigência é que o processamento ocorra praticamente em tempo real**
- Variedade : **relacionado aos diferentes tipos de fontes dos dados**
- Veracidade : **averiguar falhas nos dados e corrigí-las, garantindo a integridade e confiabilidade dos dados**
- Valor : **capacidade de extrair informações significativas dos dados analisados**

## Volume

Classificações de tipos de origem de dados :

- Dados estruturados : **dados armazenados em formatos de tabela(linhas e colunas)**
- Dados semiestruturados : **dados armazenados em conjuntos de pares de chave-valor**
- Dados não estruturados : **podem possuir diversos diversos formatos, não possuem formato consistente**

Para suprir essas necessidades a Amazon possui o **Amazon S3**, serviço de armazenamento em **Nuvem** com uma interface simples e que possui uma infraestrutura segura e altamente escalável.

Tipos de repositórios:

- Data Lake : **repositório central de dados semi e não estruturados em que os dados podem ser armazenados sem serem tratados previamente**
- Data Warehouse : **repositório central de dados estruturados em que, antes de serem armazenados, os dados precisam ser transformados em tabelas**

## Apache Hadoop

Por meio de uma arquitetura de processamento distribuído, mapeia tarefas para um cluster de computadores fazerem seu processamento. Os servidores do cluster utilizam o **Hadoop Distributed File System(HDFS)** para armazenar os dados que serão processados.

Para implementar frameworks **Hadoop** existe o serviço **AWS** chamado **Amazon EMR**. Sua principal funcionalidade é realizar ingestão de dados de diferentes origens à qualquer velocidade.

O **Amazon EMR** possui 2 sistemas de arquivos: **HDFS** e **Elastic MapReduce File System(EMRFS)**.

### <div align="center">HDFS</div>

Sistema de arquivo de **armazenamento distribuído** que permite que os arquivos sejam armazenados em servidores em paralelo.

### <div align="center">Amazon EMRFS</div>

Diferente do **HDFS**, não é preciso copiar dados para o cluster antes de transformar e analisar os dados. Economizando tempo e aumentando a agilidade.

## Velocidade

A velocidade em que os dados são gerados é cada vez maior e para isso é necessário que o processamento de dados consiga acompanhar essa velocidade, caso contrário haverá lentidão no sistema e consequentemente perda de rendimento.

O processamento de dados envolve os métodos utilizados para coletar dados e encaminhá-los à mecanismos analíticos ou de armazenamento.

### <div align="center">Processamento em batch</div>

Utilizado quando o processamento dos dados é realizado em determinados intervalos, geralmente o volume desses dados é grande. Existem dois tipos :

- Processamento em batch Programado : **realizado periodicamente, uma vez por dia ou uma vez por semana por exemplo, aumentando a previsibilidade da velocidade de processamento**
- Processamento em batch Periódico : **realizado quando os dados atingem uma quantidade específica, ou seja, não há regularidade na sua execução**

### <div align="center">Processamento em stream</div>

O processamento é realizado continuamente, em praticamente tempo real. É utilizado quando há necessidade de prontidão da análise dos dados. Existem dois tipos :

- Processamento quase em tempo real : **processamento de dados de streaming em pequenos batches individuais. Os batches são processados em minutos após a geração dos dados**
- Processamento em tempo real : **processamento de dados de streaming em batches individuais muito pequenos. Os batches são processados em milissegundos após serem gerados**

Para o processamento em tempo real podemos utilizar o **Amazon Kinesis**.

## Variedade

A variedade dos dados está relacionada à ampla variedade dos dados.

- Bancos de dados OLTP : **bancos de dados de processamento de transações on-line, caracterizados por um grande número de operações**
- Banco de dados OLAP : **banco de dados de processamento analítico, caracterizado por baixo número de operações**

Se usarmos indexação baseada em linha, é recomendável utilizar o **Amazon RDS**, caso a indexação seja colunar, é recomendável o uso do **Amazon Redshift**.

## Veracidade

É a área relacionada à garantia de precisão, consistência dos dados e confiabilidade dos dados.

Um dos fatores mais importantes da **Veracidade** é a **limpeza de dados**, esse é o processo de detecção e correção de corrupções/irregularidades nos dados. Alguns tipos de limpeza são caracterizadas por :

- Manter os dados em seu formato bruto apenas aplicando as regras empresariais
- Normalizar os dados, agregando e substituindo valores, regularizando assim todas as entradas

## Serviços AWS no processo de ETL

Os dois principais serviços de transformação de dados são o **Amazon EMR** e o **AWS Glue**.

### <div align="center">Amazon EMR</div>

É um serviço mais complexo e necessita que a equipe possua maior conhecimento técnico. Entretanto, ele permite maior robustez e é recomendado para operações e uploads mais intensivos.

### <div align="center">AWS Glue</div>

Oferece uma experiência mais simplificada que o **Amazon EMR**, não exige tanto conhecimento técnico. É indicado para tarefas mais simples de **ETL**.

## Valor

De nada adianta termos milhões de terabytes de dados se eles não significam nada para o usuário. Para isso é necessário extrair o **Valor** desses dados por meio de análises e dashboards.

Existem duas categorias de análise de dados :

- Análise de informações : **como o nome indica, é analisar os dados para encontrar o valor contido neles**
- Análise operacional : **semelhante à análise de informações, porém ela se concentra nas operações digitais da organização**

Essas duas categorias de análise possuem diferentes tipos de análise, sendo eles :

- Análise Descritiva : **também chamada de "mineração de dados", é a forma mais antiga e mais comum de análise de dados. Envolve comparar valores históricos para analisar o que aconteceu**
- Análise Diagnóstica : **envolve comparar dados coletados nas análises descritivas com outros tipos de dados para identificar a provável causa das mudanças dos dados**
- Análise Preditiva : **envolve prever o que provavelmente vai acontecer no futuro tendo como base os dados do que aconteceu no passado**
- Análise Prescritiva : **envolve analisar dados históricos e previsões para pensarmos em formas de contornar as situações e atingir os objetivos**
- Análise Cognitiva : **utiliza o deep learning(uma forma de análise artificial) juntamente da análise preditiva para tomar decisões e medidas**

## Resumindo

- Onde armazenar dados :
  - Amazon S3
- Como realizar armazenamento :
  - AWS Glue
  - Amazon RDS
  - Amazon DynamoDB
  - Amazon Redshift
- Processamento em streaming :
  - Amazon Kinesis
- Processamento em batch :
  - Amazon EMR
  - AWS Glue
- Datastores analíticos :
  - Amazon RDS
  - Amazon Redshift
- Relatórios e painéis :
  - Operacional :
    - Kibana
    - Amazon Elasticsearch Service
  - Painéis e visualizações :
    - Amazon QuickSight

## Amazon Kinesis

### <div align="center">Amazon Kinesis Streams</div>

Usado para coletar, processar e analisar dados em tempo real. Gerar análises sobre esses dados e responder em tempo real.

### <div align="center">Amazon Kinesis Analytics</div>

Processa e analisa dados em streaming com o padrão **SQL** sem a necessidade de novas linguagens de programação ou frameworks. Por meio de um serviço chamado **AWS CloudWatch**, é possível transformar os dados do **Kinesis Analytics** em dados legíveis para o usuário.

## Amazon Elastic MapReduce(EMR)

Features :

- Permite executar e escalar facilmente clusters **Hadoop**
- Integração com outros serviços AWS
- Compatibilidade com Big Data

## Amazon Athena

É serverless, não há a necessidade de configurar/gerenciar um servidor ou data warehouse. É um serviço de consulta de dados.

## Amazon Quicksight

É uma ferramenta de **BI(Business Intelligence)** fácil de usar e de baixo custo. Possui um painel/dashboard totalmente interativo que permite que o usuário escolha o modo que deseja visualizar os dados assim como a maneira como esses dados estarão organizados.

## Amazon IoT Analytics

É um serviço totalmente gerenciado de análise de dados **IoT(Internet of things)**, ele envolve processar, enriquecer, armazenar e analisar/visualizar os dados. O serviço possui 5 componentes :

- Channel : **ponto de entrada de dados**
- Pipeline : **etapa de filtragem/transformação dos dados**
- DataStore : **armazena os dados filtrados, permite querys rápidas**
- DataSet : **Data Stores utilizando SQL, criado com foco na análise dos dados. É possível visualizar utilizando o Amazon QuickSight**
- Jupyter Notebook : **ferramenta de análise de dados utilizando Python e bibliotecas**

## Amazon Redshift

É um data warehouse colunar totalmente gerenciado que utiliza ML para melhorar a eficiência das querys e do armazenamento. Possui integração com APIs, ferramentas de Data Science e com outros Databases.
