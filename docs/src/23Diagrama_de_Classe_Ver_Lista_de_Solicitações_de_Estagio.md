### UC.23 - Ver Lista de Solicitações de Estágio (Professor)

```mermaid
%% Diagrama de Classe para UC.23 - Ver Lista de Solicitações de Estágio (Professor)
classDiagram
    class PainelProfessor {
        <<boundary>>
        +visualizarSolicitacoesEstagio()
    }

    class EstagioConsultaController {
        <<control>>
        +listarPorCurso()
        +filtrarPorStatus()
    }

    class SolicitacaoEstagio {
        <<entity>>
        +aluno: String
        +empresa: String
        +status: String
        +dataEnvio: Date
    }

    class SolicitacaoEstagioCollection {
        <<entity collection>>
        +buscarTodas()
        +filtrar()
    }

    PainelProfessor --> EstagioConsultaController : requisita dados
    EstagioConsultaController --> SolicitacaoEstagioCollection : consulta
    SolicitacaoEstagioCollection --> SolicitacaoEstagio : retorna dados
