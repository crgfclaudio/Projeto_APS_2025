```mermaid
sequenceDiagram
    actor Professor

    participant PainelProfessor        as PainelProfessor (Boundary)
    participant SolicitacaoController  as SolicitacaoController (Control)
    participant SolicitacaoCollection  as SolicitacaoCollection (EntityCollection)
    participant Solicitacao            as Solicitacao (Entity)

    Professor->>PainelProfessor: visualizarSolicitacao(idSolicitacao)
    PainelProfessor->>SolicitacaoController: visualizarSolicitacao(idSolicitacao)
    SolicitacaoController->>SolicitacaoCollection: buscarPorId(idSolicitacao)
    SolicitacaoCollection-->>SolicitacaoController: solicitacao
    SolicitacaoController-->>PainelProfessor: exibirDetalhes(solicitacao)

    alt Aprovar
        Professor->>PainelProfessor: aprovar(idSolicitacao)
        PainelProfessor->>SolicitacaoController: aprovarSolicitacao(idSolicitacao)
        SolicitacaoController->>SolicitacaoController: registrarDecisao("Aprovada")
        SolicitacaoController->>SolicitacaoCollection: atualizarStatus(idSolicitacao, "Aprovada")
        SolicitacaoCollection-->>SolicitacaoController: confirmacao
        SolicitacaoController-->>PainelProfessor: mostrarConfirmacao("Solicitação aprovada")
    else Rejeitar
        Professor->>PainelProfessor: rejeitar(idSolicitacao)
        PainelProfessor->>SolicitacaoController: registrarDecisao(idSolicitacao, "Rejeitada")
        SolicitacaoController->>SolicitacaoCollection: atualizarStatus(idSolicitacao, "Rejeitada")
        SolicitacaoCollection-->>SolicitacaoController: confirmacao
        SolicitacaoController-->>PainelProfessor: mostrarConfirmacao("Solicitação rejeitada")
    end
