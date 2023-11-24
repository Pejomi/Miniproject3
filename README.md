# Mini Project 3
### af Jonas, Peter & Mie


Vi har udarbejdet vores løsning med udgangspunkt i eksamens projektet. I løsningen er implementeret to microservices:
- consumer-service
- order-service

Løsningen er baseret på følgende user story:
 *"As a consumer, I want to create an order, so that I can get food from a restaurant.".*

### Anvendelse af Kafka
Kafka er anvendt til at kommunikere mellem de to microservices. I løsningen er der anvendt en topic, som hedder "order_topics". Når der oprettes en ny ordre, bliver der sendt en besked til topic'en, som bliver modtaget af order-service. Order-service gemmer ordren i databasen og sender en besked tilbage til consumer-service, som bekræfter, at ordren er gemt.

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
