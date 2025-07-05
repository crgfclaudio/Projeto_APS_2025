### UC.15 - Ver Lista de Solicitações (Professor)

```mermaid
%% Diagrama de Classe para UC.15 - Ver Lista de Solicitações (Professor)
classDiagram
    class PainelProfessor {
        <<boundary>>
        +visualizarSolicitacoes()
    }

    class SolicitacaoController {
        <<control>>
        +listarSolicitacoes()
        +filtrar()
    }

    class Solicitacao {
        <<entity>>
        +tipo: String
        +aluno: String
        +status: String
    }

    class SolicitacaoCollection {
        <<entity collection>>
        +buscarPorCurso()
        +filtrar(filtros)
    }

    PainelProfessor --> SolicitacaoController : requisita dados
    SolicitacaoController --> SolicitacaoCollection : busca/filtra
    SolicitacaoCollection --> Solicitacao : retorna lista
