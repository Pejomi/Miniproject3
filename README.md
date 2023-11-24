# Mini Project 3

### af Jonas, Peter & Mie

Vi har udarbejdet vores løsning med udgangspunkt i eksamens projektet. I løsningen er implementeret to microservices:

- consumer-service
- order-service

Løsningen er baseret på følgende user story:
*"As a consumer, I want to create an order, so that I can get food from a restaurant.".*

### Installation af Kafka og Zookeeper

Vi bruger Kafka og Zookeeper til at kommunikere mellem vores microservices. Vi har valgt at installere Kafka og
Zookeeper lokalt på vores maskiner.
Vi har opsat det med en docker compose fil som i root folderen [her](docker-compose.yml).
Brug følgende kommandoer for at starte Kafka og Zookeeper i docker:
`docker-compose up -d`

### Opsætning af databasen

Vi har valgt at bruge en MySQL database til vores løsning. Vi har opsat databasen i docker med følgende kommando:
`docker run -d --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root mysql:latest`

Følgende 2 schemas skal oprettes i databasen:

- consumer_service
- order_service

### Anvendelse af Kafka

Kafka er anvendt til at kommunikere mellem de to microservices. I løsningen er der anvendt en topic, som hedder "
order_topics". Når der oprettes en ny ordre, bliver der sendt en besked til topic'en, som bliver modtaget af
order-service. Order-service gemmer ordren i databasen og sender en besked tilbage til consumer-service, som bekræfter,
at ordren er gemt. Følgende besked bliver logget i consumer-service:
`Order event received in stock service => OrderEvent(message=order status is in pending state, status=PENDING, orderDto=OrderDto(orderId=null, consumerId=hej, name=hej, price=10.0))`

Nedenfor ses et udsnit af koden fra order-service, som viser, hvordan der sendes en besked til topic'en.

```
public void sendMessage(OrderEvent event){
        LOGGER.info(String.format("Order event => %s", event.toString()));

        // create Message
        Message<OrderEvent> message = MessageBuilder
                .withPayload(event)
                .setHeader(KafkaHeaders.TOPIC, topic.name())
                .build();
        kafkaTemplate.send(message);
    }

```

Herunder ses et udsnit af koden fra consumer-service, som viser, hvordan der lyttes efter beskeder fra topic'en.

```
public void consume(OrderEvent event){
    LOGGER.info(String.format("Order event received in stock service => %s", event.toString()));
    // save the order event into the database
}

```

### Teknologier

Følgende teknologier er anvendt i løsningen:

- Spring Boot
- Spring Data JPA
- Spring Data REST
- Spring Cloud Stream
- Lombok
- MapStruct
- Kafka
- Maven
- MySQL
- Docker
