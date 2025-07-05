### UC.06 - Ver Lista de Atividades Vinculadas (Aluno)

```mermaid
sequenceDiagram
    actor Aluno
    participant PainelAluno as PainelAluno (Boundary)
    participant AtividadeController as AtividadeController (Control)
    participant AtividadeCollection as AtividadeCollection (EntityCollection)
    participant DB as PostgreSQL

    %% 1. Ação do aluno
    Aluno->>PainelAluno: visualizarAtividades()
    PainelAluno->>AtividadeController: GET /atividades?aluno_id=id

    %% 2. Consulta no backend
    AtividadeController->>AtividadeCollection: buscarPorAluno(idAluno)
    AtividadeCollection->>DB: SELECT * FROM atividades WHERE aluno_id = ?

    %% 3. Retorno e exibição
    DB-->>AtividadeCollection: lista de atividades
    AtividadeCollection-->>AtividadeController: atividades do aluno
    AtividadeController-->>PainelAluno: retornar lista
    PainelAluno-->>Aluno: exibir atividades com status
