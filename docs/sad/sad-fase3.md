# SAD - Software Architecture Document (Fase 3)

## 1. Introdução
Este documento reflete o estado atual (Fase 3 - Nuvem e Microsserviços) do EduVerse.O sistema superou os riscos iniciais de acoplamento com o banco de dados  migrando de um modelo N-Tier para uma estrutura Hexagonal (Ports and Adapters), garantindo a proteção das regras de negócio atreladas à Inteligência Artificial.

## 2. Visão de Implantação
A arquitetura foi desenhada para a nuvem sob um modelo PaaS. Os containers da aplicação (`Core API` e `Sync Worker`) são independentes (stateless) para permitir escalabilidade horizontal rápida. A persistência de dados críticos segue em PostgreSQL, também gerenciado na nuvem.

## 3. Qualidade e Resiliência (Trade-offs Assumidos)
Conforme priorizado nos Requisitos Não Funcionais (RNFs), as decisões arquiteturais sacrificaram parte da simplicidade inicial em prol de alta resiliência e baixa latência:
* O uso de Comunicação Assíncrona via filas evita bloqueios no Core.
* O padrão Circuit Breaker blinda a aplicação contra falhas externas.
* O sacrifício assumido foi a complexidade operacional, mitigada pela utilização de serviços gerenciados (Cloud Native).
