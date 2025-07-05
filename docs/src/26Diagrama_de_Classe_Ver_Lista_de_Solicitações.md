### UC.26 - Ver Lista de Solicitações

```mermaid
classDiagram
    class PainelProfessor {
        <<boundary>>
        +visualizarSolicitacoes()
        +aplicarFiltro(tipo, status, aluno, curso)
        +ordenar(criterio)
    }

    class SolicitacoesController {
        <<control>>
        +fetchSolicitacoes()
        +filtrarSolicitacoes(params)
        +ordenarSolicitacoes(criterio)
    }

    class Solicitacao {
        <<entity>>
        +id: String
        +nomeAluno: String
        +tipo: String
        +dataEnvio: Date
        +status: String
    }

    class SolicitacaoCollection {
        <<entity collection>>
        +solicitacoes: List<Solicitacao>
        +buscarTodas()
        +filtrar(params)
        +ordenar(criterio)
    }

    PainelProfessor --> SolicitacoesController : requisitarLista()
    SolicitacoesController --> SolicitacaoCollection : buscarTodas()
    SolicitacaoCollection --> Solicitacao : retornaDados
    SolicitacoesController -->> PainelProfessor : entregarLista(List<Solicitacao>)

