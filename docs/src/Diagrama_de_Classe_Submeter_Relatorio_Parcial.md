### UC.4 - Submeter Relatório Parcial
```mermaid
classDiagram
    class EnvioRelatorioForm {
        <<boundary>>
        +arquivo: PDF
    }

    class SubmissaoController {
        <<control>>
        +validarDocumento()
        +salvarRelatorio()
    }

    class Relatorio {
        <<entity>>
        +tipo: String
        +dataEnvio: Date
        +arquivo: File
    }

    class RelatorioCollection {
        <<entity collection>>
        +adicionar(Relatorio)
        +validarDuplicidade()
    }

    EnvioRelatorioForm --> SubmissaoController : envia
    SubmissaoController --> Relatorio : instancia novo
    SubmissaoController --> RelatorioCollection : salva
