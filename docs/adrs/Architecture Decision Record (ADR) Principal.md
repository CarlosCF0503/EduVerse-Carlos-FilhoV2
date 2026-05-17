Título da Decisão: ADR 001 - Adoção de Arquitetura Hexagonal (Ports and Adapters) para Isolamento do Domínio frente ao LMS Legado.

Status: Aceita

Contexto (Conflito de Forças):
O EduVerse possui um requisito arquitetural restritivo: ele não substitui a infraestrutura da instituição, mas coexiste e sincroniza dados continuamente com o LMS atual (Moodle).
Durante a modelagem inicial, identificou-se que a adoção do modelo tradicional N-Tier geraria um gargalo crítico e dívida técnica imediata (conforme alertado pela literatura de Pressman). No modelo N-Tier, o fluxo de dependência flui de cima para baixo (Apresentação -> Negócio -> Dados). Se a camada de negócio depender diretamente da estrutura de dados do Moodle ou do framework do banco local, qualquer mudança na API do LMS quebrará as regras do nosso motor de Inteligência Artificial. Isso fere o princípio do isolamento do domínio, tornando o sistema engessado, difícil de testar de forma isolada e altamente acoplado a uma infraestrutura que não controlamos.

Decisão Estrutural:
Decidimos implementar a Arquitetura Hexagonal (Ports and Adapters) para o core do EduVerse.
A estrutura se organizará da seguinte forma:

Core Domain (Centro do Hexágono): Conterá estritamente as entidades de negócio puras (Estudante, Trilha, Avaliação) e os Casos de Uso (ex: GerarTrilhaAdaptativaUseCase), sem depender de nenhum framework web ou ORM.

Ports (Portas): O domínio expõe interfaces (contratos). Por exemplo, uma porta de saída (Driven Port) chamada ILmsRepository definirá o que o EduVerse precisa saber sobre notas, sem se importar de onde vem.

Adapters (Adaptadores): Os componentes de infraestrutura implementarão essas portas. Criaremos um MoodleAdapter (Driven Adapter) e um PostgresAdapter. Na camada de entrada (Driving Adapters), teremos os Controladores REST da API e o serviço de Worker de sincronização.

Consequências:
O domínio do EduVerse fica completamente isolado e protegido. A integração externa ocorre de forma modularizada, garantindo flexibilidade, baixo acoplamento e permitindo que as decisões de infraestrutura e persistência sejam postergadas ou alteradas sem impacto na lógica de negócio principal do sistema.
