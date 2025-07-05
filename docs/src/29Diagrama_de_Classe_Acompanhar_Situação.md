### UC.29 - Acompanhar Situação

```mermaid
%% Diagrama de Classe para UC.29 – Acompanhar Situação
classDiagram
    class PainelProfessor {
        <<boundary>>
        +selecionarAcompanhamento(id)
        +visualizarDetalhes()
    }

    class AcompanhamentosController {
        <<control>>
        +fetchDetalhes(id)
        +fetchHistorico(id)
    }

    class Acompanhamento {
        <<entity>>
        +id: String
        +nomeAluno: String
        +tipo: String
        +dataInicio: Date
        +dataTerminoPrevisto: Date
        +planoTrabalho: String
        +status: String
    }

    class HistoricoAtualizacao {
        <<entity>>
        +data: Date
        +descricao: String
    }

    class AcompanhamentoCollection {
        <<entity collection>>
        +buscarPorId(id)
    }

    class HistoricoCollection {
        <<entity collection>>
        +buscarPorAcompanhamento(id)
    }

    PainelProfessor --> AcompanhamentosController : selecionarAcompanhamento(id)
    AcompanhamentosController --> AcompanhamentoCollection : buscarPorId(id)
    AcompanhamentoCollection --> Acompanhamento : retornaDados()
    AcompanhamentosController --> HistoricoCollection : buscarPorAcompanhamento(id)
    HistoricoCollection --> HistoricoAtualizacao : retornaHistorico()
    AcompanhamentosController --> PainelProfessor : exibirDetalhes(Acompanhamento, List<HistoricoAtualizacao>)


    PainelProfessor --> TCCController : envia revisão
    TCCController --> Revisao : instancia
    TCCController --> RevisaoCollection : armazena
