

# Projeto prático em Sistemas Distribuídos com Apache Kafka. 

## Apresentação

**Rafael Figueredo Guimarães** <br />
**Unviversidade Federal de Campina Grande - UFCG** <br />
**Programa de Pós-Graduação em Ciência da Computação**


Este trabalho tem como objetivo realizar experimentos práticos com a ferramenta Apache Kafka, buscando entender suas principais aplicações e caracteristicas que envolvem a disciplina de Sistemas Distribuídos.

## Apache Kafka: Overview

### Apache Kafka: Definição

De acordo com a documentação [[1]](https://kafka.apache.org/documentation/), Apache Kafka é uma plataforma de streaming de eventos distribuída de código aberto usada por milhares de empresas para pipelines de dados de alto desempenho, análise de streaming, integração de dados e aplicativos de missão crítica.

### Casos de uso

Conforme  a documentação, a essencia do Apache Kafka consiste em *streaming de eventos.*

> ***Event streaming*** é a prática de captura de dados em tempo real a partir de alguma fonte (ex. banco de dados, sensor, dispositivos móveis, serviços em nuvem). Esses eventos são salvos e duraveis para posterior processamento em diferentes destinos. 

Saiba mais sobre aplicações do Apache Kafka em [[2]](https://kafka.apache.org/documentation/#intro_usage).

O Apache Kafka combina três elementos chave em sua dinâmica de funcionamento: 

1. Publicar (modo escrita) e assinar (modo de leitura) fluxos de eventos, incluindo importação/exportação contínua de seus dados e de outros sistemas. 
2. Armazenar fluxos de eventos de forma durável e confiável. 
3. Processar fluxos de eventos à medida que ocorrem ou retrospectivamente.

### Principais Caracteristicas:

- Baixa Latência (refere-se ao tempo de deslocamento de uma mensagem na rede.)
- Alto Throughput (refere-se ao volume médio de dados que podem passar pela rede em um período específico)
- Escalável
- Alta disponibilidade
- Consegue armazenar mensagens
- Interoperabilidade com outros sistemas.


## Principais conceitos relacionados ao Apache Kafka. 

- **Mensagem ou evento:** Um evento registra o fato de que “algo aconteceu”.
- **Produtores:** são aplicações clientes que publicam (escrevem) eventos no Kafka.
- **Consumidores:** são aplicações que lêem e processam esses eventos. (Produtores e consumidores são agnósticas entre si.)
- **Tópicos:** Um tópico é semelhante a uma pasta em um sistema de arquivos e os eventos são os arquivos dessa pasta. Os eventos são organizados e armazenados de forma durável em tópicos.
- **Broker:**  É o servidor do Kafka, responsável por receber as mensagens dos produtores, escrever as mensagens no disco (partição) e disponibilizar para os consumidores [[3]](https://blog.dp6.com.br/apache-kafka-o-que-%C3%A9-e-como-funciona-300a5736e388#:~:text=Broker%3A%20%C3%89%20o%20servidor%20do,ler%20as%20mensagens%20do%20Kafka.)
- **Partições:** Os tópicos são particionados, o que significa que um tópico está espalhado por vários “_buckets_” localizados em diferentes brokers Kafka.


## Como o Kafka garante alta disponibilidade?

### Fator de Replicação

Um das principais caracteristicas do kafka está relacionada a sua tolerancia a falhas.  As máquinas falham e muitas vezes não podemos prever quando isso vai acontecer. O Kafka foi projetado tendo a replicação como um recurso principal para resistir a essas falhas, mantendo o tempo de atividade e a precisão dos dados.

> A replicação de dados ajuda a evitar a perda de dados gravando os mesmos dados em mais de um _broker_.

Um fator de replicação de `1` significa que não há replicação. É usado principalmente para fins de desenvolvimento e deve ser evitado em clusters Kafka de teste e produção.

Um fator de replicação de `3` é um fator de replicação comumente usado, pois fornece o equilíbrio certo entre a perda do corretor e a sobrecarga de replicação.

# Experimento Prático

### Criando uma instancia local do kafka. 

1. Para criar uma instancia do kafka localmente, clone este repositório.

~~~bash
$ git clone https://github.com/rafaelfigueredog/kafka-experiments.git
~~~

2.  Entre no pasta do repositório usando os seguintes comandos: 

~~~bash
$ cd kafka-experiments
~~~

2. Em seguida, rode o comando abaixo para subir a instancia do `Kafka` em sua máquina. (*Assumindo que você possui o `docker` instalando em sua máquina.*)

	**Obs:** caso, não possua o docker instalado em sua máquina siga a [documentação oficial](https://docs.docker.com/engine/install/) para realizar instalação. 

~~~bash
$ docker compose up
~~~


Após a realização dos passos acima teremos três containeiners estarão ativos: 

1. **Kafka:** É o próprio serviço do kafka. 
2. **Zookeeper:** É um serviço de coordenação distribuída que ajuda a gerenciar um grande conjunto de hosts.
3. **Confluent Control Center.**  É uma interface gráfica desenvolvida pela **Confluent** empresa que mantém o kafka.  

### Acessando o container do kafka. 

1. Para listar os seviços ativos rode o comando `docker compose ps`. 

![image](https://github.com/user-attachments/assets/55d090ff-29be-4268-a853-58740ff64396)

2. Para acessar o serviço do kafka rode o seguinte commando. 

~~~bash
$ docker exec -it kafka bash
~~~

### Criando o primeiro tópico. 

1. Ao acessar o container do kafka usando o comando `docker exec -it kafka bash`, execute o comando abaixo criar o primeiro tópico.

~~~bash
$ kafka-topics --create --topic=experiment  --bootstrap-server=localhost:9092 --partitions=3
~~~

O comando acima cria um novo tópico chamado "experiment" no Apache Kafka. Esse tópico terá 3 partições e se conectará ao servidor Kafka que está rodando na máquina local, na porta 9092.

**Resultado esperado:**

~~~bash
Created topic experiment.
~~~

### Analisando parametros do nosso tópico. 

1. Ainda dentro do container do kafka, o comando abaixo descreve visualizar as propriedades do nosso tópico.

~~~bash
$ kafka-topics --bootstrap-server=localhost:9092 --topic=experiment --describe
~~~

![image](https://github.com/user-attachments/assets/9180b0bb-d463-41ff-99f2-fda8d8199977)

Esse comando permite a visualização do nome do tópico, indica o lider e o _fator de replicação_. 
