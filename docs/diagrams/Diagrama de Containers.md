graph TD
    %% Atores
    Estudante(["Estudante"])
    Gestao(["Coordenador / Admin TI"])
    %% Sistema Externo
    LMS["LMS Institucional (Moodle)"]
    BD_Ext["Repositório de Conteúdos"]
    %% Sistema EduVerse (Boundary)
    subgraph EduVerse [Sistema EduVerse]
        direction TB
        WebApp["Web/Mobile App\n(SPA)"]
        API["EduVerse Core API\n(Node.js)"]
        Worker["LMS Sync Worker\n(Serviço de Background)"]
        DB[("Database Central\n(PostgreSQL)")]
    end
    %% Relações Internas
    Estudante -->|Acessa interface via| WebApp
    Gestao -->|Acessa painel via| WebApp
    WebApp -->|Faz requisições REST/GraphQL para| API
    API -->|Lê e Grava dados do Domínio| DB
    Worker -->|Sincroniza dados com BD local| DB
    %% Relações Externas
    API -->|Busca metadados e conteúdos| BD_Ext
    Worker <-->|Consome Webhooks e Sincroniza Notas/Matrículas via API externa| LMS
    %% Estilos C4
    classDef ator fill:#08427b,stroke:#052e56,color:#fff;
    classDef container fill:#438dd5,stroke:#3b7ebf,color:#fff;
    classDef db fill:#2f6b9c,stroke:#255780,color:#fff;
    classDef externo fill:#999999,stroke:#6b6b6b,color:#fff;
    class Estudante,Gestao ator;
    class WebApp,API,Worker container;
    class DB db;
    class LMS,BD_Ext externo;
