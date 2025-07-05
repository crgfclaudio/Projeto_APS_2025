### UC.27 - Aprovar Pedido

```mermaid
%% Diagrama de Classe para UC.27 – Aprovar Pedido
classDiagram
    class PainelProfessor {
        <<boundary>>
        +visualizarSolicitacoes()
        +selecionarSolicitacao(id)
        +aprovarSolicitacao()
        +rejeitarSolicitacao()
    }

    class RequestController {
        <<control>>
        +fetchSolicitacoesPendentes()
        +getDetalhesSolicitacao(id)
        +approveRequest(id)
        +rejectRequest(id)
        +notifyAluno(solicitacao)
    }

    class Solicitacao {
        <<entity>>
        +id: String
        +nomeAluno: String
        +tipo: String
        +detalhes: String
        +dataEnvio: Date
        +status: String
    }

    class SolicitacaoCollection {
        <<entity collection>>
        +buscarPendentes()
        +buscarDetalhes(id)
        +updateStatus(id, status)
    }

    PainelProfessor --> RequestController : visualizarSolicitacoes()
    RequestController --> SolicitacaoCollection : buscarPendentes()
    SolicitacaoCollection --> RequestController : retornaLista
    RequestController --> PainelProfessor : exibirLista

    PainelProfessor --> RequestController : selecionarSolicitacao(id)
    RequestController --> SolicitacaoCollection : buscarDetalhes(id)
    SolicitacaoCollection --> RequestController : retornaDados
    RequestController --> PainelProfessor : exibirDetalhes

    PainelProfessor --> RequestController : aprovarSolicitacao()
    RequestController --> SolicitacaoCollection : updateStatus(id, "Aprovada")
    SolicitacaoCollection --> RequestController : confirmacao
    RequestController --> PainelProfessor : notificarAluno()
