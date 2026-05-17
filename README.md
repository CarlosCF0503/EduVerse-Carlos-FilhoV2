# EduVerse - Plataforma Educacional Inteligente

## Visão Executiva
O EduVerse visa solucionar a limitação dos métodos de ensino tradicionais em identificar e suprir as lacunas de conhecimento individuais dos alunos.A plataforma atua como uma camada de inteligência educacional que coexiste e se integra diretamente ao LMS institucional atual (ex: Moodle), não visando substituí-lo, mas sim complementá-lo. 

No estado atual (Fase 3/4 - Nuvem e Resiliência), o sistema evoluiu de um modelo monolítico para uma Arquitetura Hexagonal com forte isolamento de domínio, preparada para implantação em nuvem (Cloud Native).O foco atual é garantir escalabilidade para suportar mais de 3.000 alunos simultâneos sem degradação do serviço  e resiliência na comunicação com o sistema legado.

## Diagrama de Containers (C4 Nível 2)
Abaixo, a representação arquitetural atualizada do sistema:

```mermaid
graph TD
    Estudante(["Estudante"])
    Gestao(["Coordenador / Admin TI"])
    
    LMS["LMS Institucional (Moodle)"]
    BD_Ext["Repositório de Conteúdos"]
    
    subgraph EduVerse [Sistema EduVerse]
        direction TB
        WebApp["Web/Mobile App (SPA)"]
        API["EduVerse Core API (Node.js)"]
        Worker["LMS Sync Worker (Background)"]
        DB[("Database Central (PostgreSQL)")]
    end
    
    Estudante -->|Acessa interface via| WebApp
    Gestao -->|Acessa painel via| WebApp
    WebApp -->|Faz requisições REST/GraphQL para| API
    API -->|Lê e Grava dados do Domínio| DB
    Worker -->|Sincroniza dados com BD local| DB
    
    API -->|Busca metadados e conteúdos| BD_Ext
    Worker <-->|Consome Webhooks e Sincroniza via mensageria| LMS

    classDef ator fill:#08427b,stroke:#052e56,color:#fff;
    classDef container fill:#438dd5,stroke:#3b7ebf,color:#fff;
    classDef db fill:#2f6b9c,stroke:#255780,color:#fff;
    classDef externo fill:#999999,stroke:#6b6b6b,color:#fff;
    class Estudante,Gestao ator;
    class WebApp,API,Worker container;
    class DB db;
    class LMS,BD_Ext externo;
```
## Documentação Arquitetural
Nossas decisões e detalhamentos estão versionados nos seguintes documentos:

* [SAD - Software Architecture Document](./docs/sad/sad-fase3.md)
* [ADR 0001 - Estratégia de Nuvem e Escalabilidade](./docs/adrs/0001-estrategia-de-nuvem-e-escalabilidade.md)
* [ADR 0002 - Padrões de Resiliência (Circuit Breaker)](./docs/adrs/0002-padrao-resiliencia.md)
* [ADR 0003 - Modelo de Comunicação (Assíncrona)](./docs/adrs/0003-modelo-comunicacao.md)
  
Como executar o projeto localmente
Como o projeto utiliza Arquitetura Hexagonal, os adaptadores podem ser facilmente mockados localmente.

Clone este repositório: git clone https://github.com/CarlosCF0503/EduVerse-Carlos-Filho.git

Acesse a pasta /gold-plating para utilizar o nosso ambiente conteinerizado pré-configurado.

Execute docker-compose up -d para subir o Banco de Dados PostgreSQL e o simulador do Worker.


