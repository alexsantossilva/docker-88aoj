
## Conclusão da disciplina - FIAP Software Engineering Development 88AOJ

### Requisitos para rodar o projeto

1. [Docker.](https://www.docker.com/)
2. [Docker Compose.](https://docs.docker.com/compose/)
3. [Webhook](https://learn.microsoft.com/en-us/microsoftteams/platform/webhooks-and-connectors/how-to/add-incoming-webhook) de um canal do Teams para enviar mensagens.
4. [Git (opcional).](https://git-scm.com/)
5. [Postman](https://www.postman.com/) ou [Insomnia](https://insomnia.rest/download) (opcional).

### Estrutura criada

1. Criado dois micros serviços com o [Spring Boot](https://spring.io/guides/gs/spring-boot/): auth e [consumer.](https://github.com/alexsantossilva/ms-consumer-88aoj)
2. Adicionado CRUD com [Swagger](https://swagger.io/tools/swagger-ui/) no micro serviço auth.
3. Criado 3 imagens docker: [PostgresSQL](https://hub.docker.com/r/alexsantossilva/88aoj-postgres), auth e [consumer](https://hub.docker.com/r/alexsantossilva/ms-consumer-spring).
4. Adicionado [Kafka](https://docs.spring.io/spring-kafka/reference/html/) como mensageria para envio das notificações dos usuários. 
5. Criado testes unitários para o micro serviço auth.
6. Criado pipeline CI utilizando [Github Actions](https://github.com/alexsantossilva/ms-auth-88aoj/actions) para o serviço de CRUD como exemplo.
7. Adicionado [Webhook no Teams](https://learn.microsoft.com/en-us/microsoftteams/platform/webhooks-and-connectors/how-to/add-incoming-webhook) para envio de notificações que foram consumidas do Kafka.

### Projetos no Github

| Projeto  | Repositório                                          |
|----------|------------------------------------------------------|
| auth     | https://github.com/alexsantossilva/ms-auth-88aoj     |
| consumer | https://github.com/alexsantossilva/ms-consumer-88aoj |
| docker   | https://github.com/alexsantossilva/docker-88aoj      |


### Imagens no Docker Hub

| Imagem   | URL                                                         |
|----------|-------------------------------------------------------------|
| auth     | https://hub.docker.com/r/alexsantossilva/ms-auth-spring                                                            |
| consumer | https://hub.docker.com/r/alexsantossilva/ms-consumer-spring |
| postgres | https://hub.docker.com/r/alexsantossilva/88aoj-postgres     |


### Dashboards adicionados no Docker Compose

| Recursos   | URL                                   | Usuário         | Pass  |
|------------|---------------------------------------|-----------------|-------|
| PgAdmin4   | http://localhost:16543                | admin@admin.com | admin |
| Kafdrop    | http://localhost:19000                |                 |       |
| Swagger    | http://localhost:8080/swagger-ui.html |                 |       |


## Como rodar o projeto


1. Clonar o projeto:
```
$ cd ~
$ mkdir project
$ cd project
$ git clone https://github.com/alexsantossilva/docker-88aoj.git
```

2. Copiar o .env.dist para .env e setar as variáveis do Webhook do Teams:
```
$ cd docker
$ cp configs/consumer/consumer.env.dist configs/consumer/consumer.env
```
Variáveis para setar `WEBHOOK_INCOMING_TEAMS` e `WEBHOOK_INCOMING_TEAMS_CHANNEL`

3. Iniciar o ambiente:
```
$ docker compose up -d
```

4. Acesse: http://localhost:8080/swagger-ui.html

5. Clique em `user-controller`

6. Agora é só ir na opção `POST` de criação de usuário e criar um usário

7. Após isso será criado o usuário e enviado uma notificação ao Kafka que será consumida pelo `consumer`
e o mesmo vai enviar uma notificação para o canal do TEAMS (o mesmo que foi setado nas variáveis de ambiente)

8. Também é possível ver os logs dos containers onde tem as flags de cada ação executada.