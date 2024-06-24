
# Experimentos práticos de sistemas distribuídos envolvendo Apache Kafka.

## Apresentação

**Autor:** Rafael Figueredo Guimarães
Unviversidade Federal de Campina Grande - UFCG

Este trabalho tem como objetivo realizar experimentos práticos com a ferramenta Apache Kafka, buscando entender suas principais aplicações e caracteristicas que envolvem a disciplina de Sistemas Distribuídos.

## Apache Kafka: Overview

### Apache Kafka: Definição

De acordo com a documentação [[1]](https://kafka.apache.org/documentation/), Apache Kafka é uma plataforma de streaming de eventos distribuída de código aberto usada por milhares de empresas para pipelines de dados de alto desempenho, análise de streaming, integração de dados e aplicativos de missão crítica.

### Casos de uso

Conforme  a documentação, a essencia do Apache Kafka consistem em *streaming de eventos.*

> ***Event streaming*** é a prática de captura de dados em tempo real a partir de alguma fonte (ex. banco de dados, sensor, dispositivos móveis, serviços em nuvem). Esses eventos sçai salvos e duraveis para posterior processamento em diferentes destinos. 

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

  

