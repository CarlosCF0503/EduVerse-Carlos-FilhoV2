# ADR 0001 - Estratégia de Nuvem e Escalabilidade

## Status
Aceita

## Contexto
O EduVerse precisa suportar mais de 3.000 alunos simultâneos sem degradação, especialmente em horários de pico como provas semanais. Uma abordagem de infraestrutura local (On-Premises) ou escalabilidade puramente vertical (aumentar a RAM do servidor) seria financeiramente inviável e tecnicamente arriscada, podendo gerar indisponibilidade.

## Decisão
Adotamos uma abordagem híbrida de Nuvem utilizando **PaaS (Platform as a Service)** para os containers principais e **SaaS/Managed Database** para persistência, focando em **Escalabilidade Horizontal**.
* A `EduVerse Core API` e o `LMS Sync Worker` serão hospedados em serviços gerenciados de containers (ex: AWS ECS com AWS Fargate).
* O banco de dados PostgreSQL  rodará como um serviço gerenciado (ex: AWS RDS).

## Consequências (Trade-offs)
* **Ganhos:** A escalabilidade horizontal permite que novas instâncias da API sejam criadas automaticamente durante a semana de provas, pagando apenas pelo consumo real. Isso corrobora com a literatura de Pressman sobre mitigação de riscos operacionais.
* **Perdas:** Ocorre o fenômeno do *Vendor Lock-in* (dependência do provedor de nuvem) e um aumento na complexidade de monitoramento (observabilidade), exigindo ferramentas adicionais para rastrear logs descentralizados.