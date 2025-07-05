### UC.28 - Ver Lista de Acompanhamentos

```mermaid
%% Diagrama de Classe para UC.28 – Ver Lista de Acompanhamentos
classDiagram
    class PainelProfessor {
        <<boundary>>
        +visualizarAcompanhamentos()
        +aplicarFiltro(tipo, curso, status, aluno)
        +ordenar(criterio)
    }

    class AcompanhamentosController {
        <<control>>
        +fetchAcompanhamentos()
        +filtrarAcompanhamentos(params)
        +ordenarAcompanhamentos(criterio)
    }

    class Acompanhamento {
        <<entity>>
        +id: String
        +nomeAluno: String
        +tipo: String
        +dataInicio: Date
        +status: String
    }

    class AcompanhamentoCollection {
        <<entity collection>>
        +buscarTodos()
        +filtrar(params)
        +ordenar(criterio)
    }

    PainelProfessor --> AcompanhamentosController : visualizarAcompanhamentos()
    AcompanhamentosController --> AcompanhamentoCollection : buscarTodos()
    AcompanhamentoCollection --> Acompanhamento : retornaDados()
    AcompanhamentosController --> PainelProfessor : entregarLista(List<Acompanhamento>)
