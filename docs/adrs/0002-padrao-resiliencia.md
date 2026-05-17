# ADR 0002 - Padrões de Resiliência (Circuit Breaker)

## Status
Aceita

## Contexto
A plataforma atua como uma camada de inteligência educacional que coexiste e se integra diretamente ao LMS institucional. ]Se o servidor do Moodle apresentar lentidão ou cair, o `LMS Sync Worker` ficaria aguardando eternamente por respostas, esgotando o pool de conexões, travando a geração de trilhas e quebrando o requisito de resposta em 2 segundos.

## Decisão
Implementamos o padrão de resiliência **Circuit Breaker** (Disjuntor) no adaptador de saída (`Driven Adapter`) responsável pela comunicação com o Moodle.
* Se a taxa de falha ou tempo de resposta do Moodle exceder o limite aceitável, o circuito se "abre" e corta a comunicação.
* Durante o estado aberto, o sistema utiliza uma estratégia de **Fallback**, carregando uma "Trilha Padrão"  para o aluno não ficar sem conteúdo.

## Consequências (Trade-offs)
* **Ganhos:** Previne falhas em cascata. O EduVerse permanece funcional e rápido mesmo se a infraestrutura legada da faculdade estiver fora do ar.
* **Perdas:** Os alunos podem receber recomendações desatualizadas (dados em cache ou trilhas genéricas) enquanto o circuito estiver aberto, reduzindo temporariamente a precisão de 80% exigida pela IA.
