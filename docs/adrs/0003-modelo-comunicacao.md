# ADR 0003 - Modelo de Comunicação

## Status
Aceita

## Contexto
Com a decisão de separar o `LMS Sync Worker` da `Core API`, precisamos definir como esses containers vão trocar dados. Uma comunicação síncrona (REST) entre eles geraria um alto acoplamento temporal, violando o princípio de isolamento que buscamos com a Arquitetura Hexagonal.

## Decisão
Adotamos um modelo de **Comunicação Híbrida**:
1. **Síncrona (REST/GraphQL):** Exclusivamente para a comunicação entre o `Web/Mobile App` e a `Core API`, pois o usuário precisa de feedback visual imediato em tarefas como a realização de avaliações em menos de 2 minutos.
2. **Assíncrona (Mensageria/Event-Driven):** Para a comunicação entre a `Core API` e o `LMS Sync Worker`. Quando a API precisar que um dado vá para o Moodle, ela publicará um evento em um Message Broker (ex: RabbitMQ/SQS).

## Consequências (Trade-offs)
* **Ganhos:** Desacoplamento extremo. Se o Worker cair, a API continua funcionando perfeitamente; as mensagens apenas ficarão na fila até o Worker voltar à vida (garantia de entrega).
* **Perdas:** Introduz o conceito de *Consistência Eventual*. O estudante pode terminar uma prova no EduVerse e levar alguns minutos até que a nota reflita no sistema oficial (Moodle), podendo gerar suporte de TI por ansiedade do usuário.
