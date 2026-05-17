graph LR
    %% Atores
    Estudante(["Estudante"])
    Coordenador(["Coordenador"])
    AdminTI(["Admin de TI"])
    %% Sistema Central
    EduVerse(("EduVerse"))
    %% Sistemas Externos (Em cinza, padrão C4)
    LMS["LMS Institucional (ex: Moodle)"]
    Repo["Repositório de Conteúdos"]
    Auth["Sistema de Autenticação"]
    Email["Sistema de Disparo de E-mails"]
    %% Relações - Atores para o Sistema
    Estudante -->|Acessa aulas, realiza avaliações e visualiza trilhas| EduVerse
    Coordenador -->|Acompanha relatórios de desempenho e métricas| EduVerse
    AdminTI -->|Gerencia integrações e configura acessos| EduVerse
    %% Relações - Sistema para Externos
    EduVerse <-->|Sincroniza matrículas, notas e o progresso das trilhas| LMS
    EduVerse -->|Busca metadados e links de recursos educacionais| Repo
    EduVerse -->|Valida credenciais e tokens de acesso usando| Auth
    EduVerse -->|Dispara e-mails transacionais| Email
    %% Classes de Estilo C4
    classDef ator fill:#08427b,stroke:#052e56,color:#fff;
    classDef sistema fill:#1168bd,stroke:#0b4884,color:#fff;
    classDef externo fill:#999999,stroke:#6b6b6b,color:#fff;
    class Estudante,Coordenador,AdminTI ator;
    class EduVerse sistema;
    class LMS,Repo,Auth,Email externo;
