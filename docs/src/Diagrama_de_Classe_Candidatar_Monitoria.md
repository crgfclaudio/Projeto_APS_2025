### UC.9 - Se Candidatar à Monitoria
```mermaid
classDiagram
    class CandidaturaForm {
        <<boundary>>
        +preencherDados()
        +anexarDocs()
    }

    class MonitoriaController {
        <<control>>
        +validarRequisitos()
        +registrarCandidatura()
    }

    class Candidatura {
        <<entity>>
        +idAluno: String
        +idMonitoria: String
        +dataEnvio: Date
        +documentos: File[]
    }

    class CandidaturaCollection {
        <<entity collection>>
        +adicionar(Candidatura)
        +verificarDuplicidade()
    }

    CandidaturaForm --> MonitoriaController : envia dados
    MonitoriaController --> Candidatura : instancia
    MonitoriaController --> CandidaturaCollection : armazena
