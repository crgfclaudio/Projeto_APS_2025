### UC.16 - Aprovar Solicitação (Professor)

```mermaid
%% Diagrama de Classe para UC.16 - Aprovar Solicitação (Professor)
classDiagram
    class PainelProfessor {
        <<boundary>>
        +visualizarSolicitacao()
        +aprovar()
        +rejeitar()
    }

    class SolicitacaoController {
        <<control>>
        +aprovarSolicitacao(id)
        +registrarDecisao()
    }

    class Solicitacao {
        <<entity>>
        +id: String
        +status: String
        +motivoRejeicao: String
    }

    class SolicitacaoCollection {
        <<entity collection>>
        +buscarPorId()
        +atualizarStatus()
    }

    PainelProfessor --> SolicitacaoController : envia ação
    SolicitacaoController --> SolicitacaoCollection : consulta e atualiza
    SolicitacaoCollection --> Solicitacao : modifica status
