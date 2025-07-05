### UC.24 - Aceitar Solicitação de Estágio (Professor)

```mermaid
%% Diagrama de Classe para UC.24 - Aceitar Solicitação de Estágio (Professor)
classDiagram
    class PainelProfessor {
        <<boundary>>
        +visualizarSolicitacao()
        +aceitarSolicitacao()
        +rejeitarSolicitacao()
    }

    class AprovarEstagioController {
        <<control>>
        +validarSolicitacao()
        +atualizarStatus()
    }

    class SolicitacaoEstagio {
        <<entity>>
        +id: String
        +status: String
        +comentario: String
    }

    class SolicitacaoEstagioCollection {
        <<entity collection>>
        +buscarPorId()
        +modificarStatus()
    }

    PainelProfessor --> AprovarEstagioController : envia decisão
    AprovarEstagioController --> SolicitacaoEstagioCollection : atualiza
