


# Projeto prático em Sistemas Distribuídos com Apache Kafka. 

**Rafael Figueredo Guimarães** <br />
**Unviversidade Federal de Campina Grande - UFCG** <br />
**Programa de Pós-Graduação em Ciência da Computação**

## Objetivo

Este trabalho tem como objetivo apresentar um estudo relacionando conceitos de **Sistemas Distribuídos** e a ferramenta **Apache Kafka**. O trabalho está subdividido em 2 seções, a primeira seção apresenta um breve resumo teorico sobre o Kafka e em seguida alguns experimentos práticos envolvendo tolerancia a falhas, replicação em sistemas distribuídos, eleição de lider. 
 
## Apache Kafka: Fundamentação Teorica

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


### Conceitos relacionados ao Apache Kafka. 

- **Mensagem ou evento:** Um evento registra o fato de que “algo aconteceu”.
- **Produtores:** são aplicações clientes que publicam (escrevem) eventos no Kafka.
- **Consumidores:** são aplicações que lêem e processam esses eventos. (Produtores e consumidores são agnósticas entre si.)
- **Tópicos:** Um tópico é semelhante a uma pasta em um sistema de arquivos e os eventos são os arquivos dessa pasta. Os eventos são organizados e armazenados de forma durável em tópicos.
- **Broker:**  É o servidor do Kafka, responsável por receber as mensagens dos produtores, escrever as mensagens no disco (partição) e disponibilizar para os consumidores [[3]](https://blog.dp6.com.br/apache-kafka-o-que-%C3%A9-e-como-funciona-300a5736e388#:~:text=Broker%3A%20%C3%89%20o%20servidor%20do,ler%20as%20mensagens%20do%20Kafka.)
- **Partições:** Os tópicos são particionados, o que significa que um tópico está espalhado por vários “_buckets_” localizados em diferentes brokers Kafka.


### Como o Kafka garante alta disponibilidade?

#### Fator de Replicação

Um das principais caracteristicas do kafka está relacionada a sua tolerancia a falhas.  As máquinas falham e muitas vezes não podemos prever quando isso vai acontecer. O Kafka foi projetado tendo a replicação como um recurso principal para resistir a essas falhas, mantendo o tempo de atividade e a precisão dos dados.

> A replicação de dados ajuda a evitar a perda de dados gravando os mesmos dados em mais de um _broker_.

Um fator de replicação de `1` significa que não há replicação. É usado principalmente para fins de desenvolvimento e deve ser evitado em clusters Kafka de teste e produção.

Um fator de replicação de `3` é um fator de replicação comumente usado, pois fornece o equilíbrio certo entre a perda do corretor e a sobrecarga de replicação.

## Experimentos Práticos

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


Após a realização dos passos acima os seguintes containeiners estarão ativos: 

1. **Broker:** É o próprio serviço do kafka. 
2. **Zookeeper:** É um serviço de coordenação distribuída que ajuda a gerenciar um grande conjunto de hosts.
3. **Confluent Control Center:**  É uma interface gráfica desenvolvida pela **Confluent** empresa que mantém o kafka.  
4. **Schema Registry:** é um processo que roda em um servidor próximo aos brokers do Kafka e fica responsável por armazenar os schemas e suas possíveis versões
5. **Kafka Connect:** é um componente gratuito e de código aberto do Apache Kafka® que serve como um hub de dados centralizado para integração simples de dados entre bancos de dados, armazenamentos de valores-chave, índices de pesquisa e sistemas de arquivos.
6. **Ksqldb Server:** Permite construir aplicativos de streaming de eventos com a mesma facilidade e familiaridade de construir aplicativos tradicionais em um banco de dados relacional.

O arquivo `docker-compose.yml` utilizado como referencia é da própria mantededora do Apache Kafka. [all in one confluent plaftorm.](https://github.com/confluentinc/cp-all-in-one/blob/7.5.0-post/cp-all-in-one/docker-compose.yml) Partindo dessa base foram criados mais 2 instancias de brokers para realização de experimentos com replicação. 

### Criando um novo tópico usando control center da confluent. 

1. Abrir console do control-center `http://localhost:9021`
3. Criar um tópico na pagina `Topics` com configurações default (essa configuração já habilita um nível equilibrado entre disponibilidade e durabilidade, com fator de replicação `3`.

### Experimento 1: Simulando falhas no broker controller? 
 
Um cluster `Kafka` pode ter vários *brokers*, mas apenas um *broker* pode ser o controlador. O processo de eleger um *broker* entre a lista de corretores em um cluster para ser o controlador ativo é chamado de **eleição de líder.**

![image](https://github.com/user-attachments/assets/0fa0fc7b-fc50-4e1f-a052-e083a877f5e0)

O `Zookeeper` deve fazer a eleição do líder quando o corretor controlador estiver inativo. O corretor controlador, por sua vez, é responsável por eleger os líderes da partição, garantindo que as leituras e gravações dos clientes no Kafka ainda possam acontecer.

1. Primeiro passo é garantir que todos os containers estão ativos `docker compose ps`
2. Em seguida abra o control center no `http://localhost:9021` e verifique o qual é o id do active controller, no menu `brokers`
3. Em seguida execute `docker compose stop <nome-do-container-broker-ativo>`
4. Após esse processo é experado que tenhamos o seguinte resultado. 

![image](https://github.com/user-attachments/assets/835583d0-7d73-4e8b-bb86-d54c53635345)


### Experimento 2: Simulando falhas no broker controller e no Zookeeper.  

Falhas no Kafka podem ocorrer se o **broker ativo** e o **Zookeeper** tiverem problemas. O Zookeeper é responsável por eleger um novo controller, e se ele estiver inativo, não haverá eleição. Isso impede que o líder da partição do tópico identifique as réplicas que devem se tornar o novo líder. Como resultado, podem ocorrer falhas e corrupção de dados. No aquivo desse experimento temos apenas um uma instancia do Zookeeper ativo, no entanto, esse problema pode ser contornado criando [replicas do zookeeper](https://hub.docker.com/_/zookeeper). 

Para realizar o experimento vamos realizar os seguintes passos.

1. Primeiro passo é garantir que todos os containers estão ativos `docker compose ps`
2. Em seguida abra o control center no `http://localhost:9021` e verifique o qual é o id do active controller, no menu `brokers`
3. Em seguida execute `docker compose stop <nome-do-container-broker-ativo>`
4. Em seguida execute `docker compose stop <nome-do-container-zookeeper>`
5. Após essas ações, mesmo com todos os outros serviços ativos, obtive o seguinte log do control center.

> Current cluster went offiline a minute ago.

### Experimento 3: Simulando falhas no Zookeeper.  

Enquanto o broker controller estiver ativo, mesmo que ocorra uma falha no Zookeeper o serviço do Kafka permanece ativo. 

Para realizar o experimento vamos realizar os seguintes passos.

1. Primeiro passo é garantir que todos os containers estão ativos `docker compose ps`
2. Em seguida execute `docker compose stop <nome-do-container-zookeeper>`
5. Após essas ações, note que ainda é possível produzir mensagens através o control center ou de alguma aplicação cliente.
