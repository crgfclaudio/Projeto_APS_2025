### UC.6 - Ver Lista de Atividades Vinculadas
```mermaid
classDiagram
    class PainelAluno {
        <<boundary>>
        +visualizarAtividades()
    }

    class AtividadeController {
        <<control>>
        +listarAtividadesAluno(idAluno)
    }

    class Atividade {
        <<entity>>
        +tipo: String
        +status: String
        +titulo: String
    }

    class AtividadeCollection {
        <<entity collection>>
        +buscarPorAluno(idAluno)
    }

    PainelAluno --> AtividadeController : requisita dados
    AtividadeController --> AtividadeCollection : consulta
    AtividadeCollection --> Atividade : retorna instâncias
    AtividadeController --> PainelAluno : retorna lista
