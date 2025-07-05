### UC.24 - Aceitar Solicitação de Estágio (Professor)

```mermaid
sequenceDiagram
    actor Professor

    participant PainelProfessor            as PainelProfessor (Boundary)
    participant AprovarEstagioController   as AprovarEstagioController (Control)
    participant SolicitacaoEstagioCollection as SolicitacaoEstagioCollection (EntityCollection)
    participant SolicitacaoEstagio         as SolicitacaoEstagio (Entity)

    Professor->>PainelProfessor: visualizarSolicitacao(idSolicitacao)
    PainelProfessor->>AprovarEstagioController: visualizarSolicitacao(idSolicitacao)
    AprovarEstagioController->>SolicitacaoEstagioCollection: buscarPorId(idSolicitacao)
    SolicitacaoEstagioCollection-->>AprovarEstagioController: solicitacao
    AprovarEstagioController-->>PainelProfessor: exibirDetalhes(solicitacao)

    alt Aprovar
        Professor->>PainelProfessor: aceitarSolicitacao(idSolicitacao)
        PainelProfessor->>AprovarEstagioController: aceitarSolicitacao(idSolicitacao)
        AprovarEstagioController->>AprovarEstagioController: validarSolicitacao(solicitacao)
        alt válida
            AprovarEstagioController->>SolicitacaoEstagioCollection: modificarStatus(idSolicitacao, "Aprovada")
            SolicitacaoEstagioCollection-->>AprovarEstagioController: confirmacao
            AprovarEstagioController-->>PainelProfessor: mostrarConfirmacao("Solicitação de estágio aprovada")
        else inválida
            AprovarEstagioController-->>PainelProfessor: mostrarErro("Solicitação inválida")
        end
    else Rejeitar
        Professor->>PainelProfessor: rejeitarSolicitacao(idSolicitacao)
        PainelProfessor->>AprovarEstagioController: rejeitarSolicitacao(idSolicitacao)
        AprovarEstagioController->>AprovarEstagioController: validarSolicitacao(solicitacao)
        alt válida
            AprovarEstagioController->>SolicitacaoEstagioCollection: modificarStatus(idSolicitacao, "Rejeitada")
            SolicitacaoEstagioCollection-->>AprovarEstagioController: confirmacao
            AprovarEstagioController-->>PainelProfessor: mostrarConfirmacao("Solicitação de estágio rejeitada")
        else inválida
            AprovarEstagioController-->>PainelProfessor: mostrarErro("Solicitação inválida")
        end
    end
