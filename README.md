

# Experimentos práticos de sistemas distribuídos envolvendo Apache Kafka.

## Apresentação

**Autor:** Rafael Figueredo Guimarães
**Instituição:** Unviversidade Federal de Campina Grande - UFCG

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


## Experimento Prático

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


