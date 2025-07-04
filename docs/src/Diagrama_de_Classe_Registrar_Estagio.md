### UC.11 - Registrar Estágio
```mermaid
classDiagram
    class EstagioForm {
        <<boundary>>
        +preencherFormulario()
        +anexarDocumento()
    }

    class EstagioController {
        <<control>>
        +validarDados()
        +registrarSolicitacao()
    }

    class Estagio {
        <<entity>>
        +idAluno: String
        +empresa: String
        +supervisor: String
        +documento: File
    }

    class EstagioCollection {
        <<entity collection>>
        +adicionar(Estagio)
        +validarRegras()
    }

    EstagioForm --> EstagioController : envia dados
    EstagioController --> Estagio : cria instância
    EstagioController --> EstagioCollection : armazena
