### UC.7 - Solicitar Professor
```mermaid
classDiagram
    class IndicacaoProfessorForm {
        <<boundary>>
        +selecionarProfessor()
    }

    class TCCController {
        <<control>>
        +vincularProfessor()
        +validarDisponibilidade()
    }

    class Professor {
        <<entity>>
        +nome: String
        +id: String
        +orientandosAtivos: int
    }

    class TCC {
        <<entity>>
        +idAluno: String
        +titulo: String
        +orientador: Professor
    }

    class ProfessorCollection {
        <<entity collection>>
        +buscarDisponiveis()
        +consultarOrientandos()
    }

    IndicacaoProfessorForm --> TCCController : envia seleção
    TCCController --> ProfessorCollection : valida professor
    TCCController --> TCC : vincula professor
