### UC.06 - Ver Lista de Atividades Vinculadas (Aluno) com Arquitetura

```mermaid
sequenceDiagram
    actor Aluno

    participant WebApp as WebApp (Apresentação)
    participant AlunoService as AlunoService (Controller)
    participant Modulos as Gerenciador de Módulos
    participant Relatorio as Módulo Relatório
    participant DB as DatabaseService
    participant DocDB as Doc DB

    %% 1. Requisição do aluno pela interface
    Aluno->>WebApp: visualizarMinhasAtividades()
    WebApp->>AlunoService: GET /atividades?aluno_id=123

    %% 2. Direcionamento da requisição
    AlunoService->>Modulos: solicitarAtividadesAluno(idAluno)
    Modulos->>Relatorio: buscarAtividadesVinculadas(idAluno)

    %% 3. Consulta ao banco de dados
    Relatorio->>DB: consultarAtividadesPorAluno(idAluno)
    DB->>DocDB: SELECT * FROM atividades WHERE aluno_id = 123
    DocDB-->>DB: listaAtividades
    DB-->>Relatorio: retornoAtividades

    %% 4. Resposta final
    Relatorio-->>Modulos: atividadesEncontradas
    Modulos-->>AlunoService: atividades do aluno
    AlunoService-->>WebApp: exibirLista(atividades)
    WebApp-->>Aluno: mostrarAtividadesComStatus

