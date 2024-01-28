# Curso Full Cycle 3.0 - Módulo Apache Kafka

<div>
    <img alt="Criado por Alcir Junior [Caju]" src="https://img.shields.io/badge/criado%20por-Alcir Junior [Caju]-%23f08700">
    <img alt="License" src="https://img.shields.io/badge/license-MIT-%23f08700">
</div>

---

## Descrição

O Curso Full Cycle é uma formação completa para fazer com que pessoas desenvolvedoras sejam capazes de trabalhar em projetos expressivos sendo capazes de desenvolver aplicações de grande porte utilizando de boas práticas de desenvolvimento.

---

## Repositório Pai
https://github.com/alcir-junior-caju/study-full-cycle-3-0

---

## Visualizar o projeto na IDE:

Para quem quiser visualizar o projeto na IDE clique no teclado a tecla `ponto`, esse recurso do GitHub é bem bacana

---

### O que é Apache Kafka
- O Apache Kafka é uma plataforma distribuída de streaming de eventos open-source que é utilizada por milhares de empresas para uma alta performance em pipeline de dados, stream de analytics, integração de dados e aplicações de missão crítica.

### O mundo do eventos
- Cada dia precisamos processar mais e mais eventos em diversos tipos de plataformas. Desde sistemas que precisam se comunicar, devices para IOT, monitoramento de aplicações, sistemas de alarmes, etc;
- Perguntas:
    - Onde salvar esses eventos?
    - Como recuperar de forma rápida e simples, de forma que o feedback entre um processo e outro, ou mesmo entre um sistemas e outro possa acontecer de forma fluida e em tempo real?
    - Como escalar?
    - Como ter resiliência e alta disponibilidade?

### Apache Kafka e seus super poderes
- Altíssimo throughput;
- Latência extremamente baixa (2ms);
- Escalável;
- Armazenamento;
- Alta disponibilidade;
- Se conecta com quase tudo;
- Bibiliotecas prontas para as mais diversas tecnologias;
- Ferramentas open-source;

### Empresas usando o Kafka
- Linkedin;
- Netflix;
- Uber;
- Twitter;
- Dropbox;
- Spotify;
- Paypal;
- Bancos...

### Funcionamento
- Producer ->;
    - Kafka;
    - 3 Broker recomendados;
- <- Consumer;

### Tópicos
- Tópico é o canal de comunicação responsável por receber e disponibilizar os dados enviados para o kafka;
- Producer ->;
    - Topic;
- <- Consumer 1;
- <- Consumer 2;
- Tópic ~= Log

### Anatomia de um registro
- Offset 0;
    - Headers;
    - Keys;
    - Value;
    - Timestamp;

### Partições
- Cada Tópico pode ter uma ou mais partições para conseguir garantir a distribuição e resiliência de seus dados;
    - Tópico;
        - Partição 1;
        - Partição 2;
        - Partição 3;

### Keys
- Partição 1 <- Consumer 1 (lento);
    - Offset 0, Offset 1; Transferência;
- Partição 2 <- Consumer 2 (rápido);
    - Offset 0; Estorno;
- Partição 3 <- Consumer 2;
    - Offset 0;
- Pode acontecer que o estorno chegue antes do que a transferência, pois a partição 1 está lento;
- Partição 1 <- Consumer 1 (lento);
    - Offset 0, Offset 1; Transferência(0) key = Movimentação Estorno (1) key = Movimentação;
- Partição 2 <- Consumer 2 (rápido);
    - Offset 0;
- Partição 3 <- Consumer 2;
    - Offset 0;

### Partições Distribuídas
- Tópico: Vendas;
    - Broker A -> Partição 1;
    - Broker B -> Partição 2;
    - Broker C -> Partição 3;
- Tópico: Vendas -> Replicator Factor = 2;
    - Broker A -> Partição 1, Partição 3;
    - Broker B -> Partição 2, Partição 1;
    - Broker C -> Partição 3, Partição 2;

### Partition Leadership
- Tópico: Vendas -> Replicator Factor = 2;
    - Broker A -> Partição 1 (Leadership), Partição 2 (Followers), Partição 4 (Followers);
    - Broker B -> Partição 1 (Followers), Partição 2 (Leadership), Partição 3 (Followers);
    - Broker C -> Partição 4 (Followers), Partição 2 (Followers), Partição 3 (Leadership);
    - Broker D -> Partição 3 (Followers), Partição 2 (Followers), Partição 4 (Leadership);
- Caso o Broker A venha a cair, o kafka procura onde temos a replica que seria no Broker B, e a cópia se torna um Leadership também;

### Producer: Garantia de entrega nas mensagens
- Producer;
    - Ack 0 (none);
    - Ack 1 (Leadership);
    - Ack -1 (All);
        - Broker A (Leadership);
        - Broker B (Follower);
        - Broker C (Follower);
    - At most once: Melhor performance. Pode perder algumas mensagens;
    - At least once: Performance moderada. Pode duplicar mensagens;
    - Exacly once: Pior performance. Exatamente uma vez;

### Producer: indepotência
- Descarta mensagens caso haja um problema de rede ou perda de mensagem, e o producer envia duas vezes a mesma mensagem;

### Consumers e Consumers groups
- Producer ->;
    - Kafka;
    - 3 Broker recomendados;
        - Tópico: Vendas;
        - Broker A -> Partição 1;
        - Broker B -> Partição 2;
        - Broker C -> Partição 3;
- Group;
    - <- Consumer A;
    - <- Consumer B;
- Sempre um consumer por partição em um grupo;

### O que é Kafka Connect
- Kafka Connect é um componente gratuito e open-source do Apache Kafka que trabalha como um Hub de dados centralizado para integrações simples entre banco de dados, key-value stores, search indexes e file systems.

### Dinâmica
- Kafka Connect ->;
    - Connector (Data Sources) MySQL;
    - Connector (Data Sources) Mongo;
    - Connector (Data Sources) Salesforce;
    - Connector (Sinks) JDBC;
    - Connector (Sinks) Elasticsearch;
    - Connector (Sinks) AWS Lambda;
- <- Apache Kafka;

### Standalone Workers
- Worker;
    - Connector A - Task 1;
    - Connector B - Task 1;

### Distributed Workers
- Kafka Connect Cluster;
    - Worker 1;
        - Connector A - Task 1 - Part 1;
        - Connector B - Task 1 - Part 1,2;
    - Worker 2;
        - Connector A - Task 2 - Part 2;
        - Connector B - Task 2 - Part 3,4;
    - Worker 3;
        - Connector A - Task 3 - Part 3;
        - Connector B - Task 3 - Part 5;

### Converters
- As tasks utilizam os `converters` para mudar o formato dos dados tanto para leitura ou escrita no kafka;
- Avro;
- Protobuf;
- JsonSchema;
- Json;
- String;
- ByteArray;

### DLQ - Dead Letter Queue
- Quando há um registro inválido, independente da razão, o erro pode ser tratado nas configurações do connector através da propriedade `errors.tolerance`. Esse tipo de configuração pode ser realizado apenas para conectores do tipo `SINK`;
- none: Faz a tarefa falhar imediatamente;
- all: Erros são ignorados e o processo continua normalmente;
    - errors.deadletterqueue.topic.name = <topicname>

### Links
- Para testes pode usar o Confluent Cloud no plano free;
