### UC.5 - Submeter Relatório Final
```mermaid
classDiagram
    class EnvioRelatorioFinalForm {
        <<boundary>>
        +arquivo: PDF
    }

    class SubmissaoFinalController {
        <<control>>
        +validarFinal()
        +registrarEnvio()
    }

    class RelatorioFinal {
        <<entity>>
        +data: Date
        +arquivo: File
        +status: String
    }

    class RelatorioCollection {
        <<entity collection>>
        +adicionar(RelatorioFinal)
        +verificarSubmissaoFinal()
    }

    EnvioRelatorioFinalForm --> SubmissaoFinalController : envia
    SubmissaoFinalController --> RelatorioFinal : cria
    SubmissaoFinalController --> RelatorioCollection : armazena
