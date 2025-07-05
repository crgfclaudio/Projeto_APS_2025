### UC.16 - Aprovar Solicitação (Professor)
```mermaid

sequenceDiagram
    actor Professor
    participant PainelProfessor as PainelProfessor (Boundary)
    participant SolicitacaoController as SolicitacaoController (Control)
    participant SolicitacaoCollection as SolicitacaoCollection (EntityCollection)
    participant Solicitacao as Solicitacao (Entity)
    participant DB as PostgreSQL

    %% Visualização inicial da solicitação
    Professor->>PainelProfessor: visualizarSolicitacao(id)
    PainelProfessor->>SolicitacaoController: visualizarSolicitacao(id)
    SolicitacaoController->>SolicitacaoCollection: buscarPorId(id)
    SolicitacaoCollection->>DB: SELECT * FROM solicitacoes WHERE id = ?
    DB-->>SolicitacaoCollection: dados da solicitação
    SolicitacaoCollection-->>SolicitacaoController: solicitacao
    SolicitacaoController-->>PainelProfessor: exibirDetalhes(solicitacao)

    %% Fluxo principal – Decisão
    alt Aprovar
        Professor->>PainelProfessor: aprovar(id)
        PainelProfessor->>SolicitacaoController: aprovarSolicitacao(id)
        SolicitacaoController->>SolicitacaoCollection: atualizarStatus(id, "Aprovada")
        SolicitacaoCollection->>DB: UPDATE solicitacoes SET status = 'Aprovada' WHERE id = ?
        DB-->>SolicitacaoCollection: OK
        SolicitacaoCollection-->>SolicitacaoController: confirmação
        SolicitacaoController-->>PainelProfessor: mostrarConfirmacao("Solicitação aprovada")
    else Rejeitar
        Professor->>PainelProfessor: rejeitar(id)
        PainelProfessor->>SolicitacaoController: rejeitarSolicitacao(id)
        SolicitacaoController->>SolicitacaoCollection: atualizarStatus(id, "Rejeitada")
        SolicitacaoCollection->>DB: UPDATE solicitacoes SET status = 'Rejeitada' WHERE id = ?
        DB-->>SolicitacaoCollection: OK
        SolicitacaoCollection-->>SolicitacaoController: confirmação
        SolicitacaoController-->>PainelProfessor: mostrarConfirmacao("Solicitação rejeitada")
    end
