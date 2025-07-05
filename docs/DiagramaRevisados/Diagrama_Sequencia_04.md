### UC.4 - Submeter Relatório Parcial com Arquitetura
```mermaid
sequenceDiagram
    actor Aluno

    participant WebApp as WebApp (Apresentação)
    participant AlunoService as AlunoService (Controller)
    participant Modulos as Gerenciador de Módulos
    participant Relatorio as Módulo Relatório
    participant DB as DatabaseService
    participant DocDB as Doc DB

    %% 1. Submissão da interface
    Aluno->>WebApp: enviarRelatorioParcial(PDF, metadata)
    WebApp->>AlunoService: POST /relatorio/parcial (dados, arquivo)

    %% 2. Encaminhamento interno
    AlunoService->>Modulos: direcionarParaModulo("relatorioParcial")
    Modulos->>Relatorio: processarSubmissaoParcial(idAluno, arquivo)

    %% 3. Verificação de duplicidade
    Relatorio->>DB: verificarDuplicidade(idAluno, tipo="parcial")
    DB->>DocDB: SELECT * FROM re*

