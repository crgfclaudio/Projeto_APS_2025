### UC.24 - Aceitar Solicitação de Estágio (Professor)

````mermaid
sequenceDiagram
    actor Professor
    participant PainelProfessor as PainelProfessor (Boundary)
    participant AprovarEstagioController as AprovarEstagioController (Control)
    participant SolicitacaoEstagioCollection as SolicitacaoEstagioCollection (EntityCollection)
    participant SolicitacaoEstagio as SolicitacaoEstagio (Entity)
    participant DB as PostgreSQL

    %% Fluxo de visualização da solicitação
    Professor->>PainelProfessor: visualizarSolicitacao(id)
    PainelProfessor->>AprovarEstagioController: visualizarSolicitacao(id)
    AprovarEstagioController->>SolicitacaoEstagioCollection: buscarPorId(id)
    SolicitacaoEstagioCollection->>DB: SELECT * FROM solicitacoes WHERE id = ?
    DB-->>SolicitacaoEstagioCollection: dados da solicitação
    SolicitacaoEstagioCollection-->>AprovarEstagioController: solicitacao
    AprovarEstagioController-->>PainelProfessor: exibirDetalhes(solicitacao)

    %% Fluxo principal - Aprovação
    alt Professor aprova
        Professor->>PainelProfessor: aceitarSolicitacao(id)
        PainelProfessor->>AprovarEstagioController: aceitarSolicitacao(id)
        AprovarEstagioController->>AprovarEstagioController: validarSolicitacao(solicitacao)
        alt Solicitação válida
            AprovarEstagioController->>SolicitacaoEstagioCollection: modificarStatus(id, "Aprovada")
            SolicitacaoEstagioCollection->>DB: UPDATE solicitacoes SET status='Aprovada' WHERE id = ?
            DB-->>SolicitacaoEstagioCollection: OK
            SolicitacaoEstagioCollection-->>AprovarEstagioController: confirmação
            AprovarEstagioController-->>PainelProfessor: mostrarConfirmacao("Solicitação aprovada")
        else Solicitação inválida
            AprovarEstagioController-->>PainelProfessor: mostrarErro("Solicitação inválida")
        end
    else Professor rejeita
        Professor->>PainelProfessor: rejeitarSolicitacao(id)
        PainelProfessor->>AprovarEstagioController: rejeitarSolicitacao(id)
        AprovarEstagioController->>AprovarEstagioController: validarSolicitacao(solicitacao)
        alt Solicitação válida
            AprovarEstagioController->>SolicitacaoEstagioCollection: modificarStatus(id, "Rejeitada")
            SolicitacaoEstagioCollection->>DB: UPDATE solicitacoes SET status='Rejeitada' WHERE id = ?
            DB-->>SolicitacaoEstagioCollection: OK
            SolicitacaoEstagioCollection-->>AprovarEstagioController: confirmação
            AprovarEstagioController-->>PainelProfessor: mostrarConfirmacao("Solicitação rejeitada")
        else Solicitação inválida
            AprovarEstagioController-->>PainelProfessor: mostrarErro("Solicitação inválida")
        end
    end
