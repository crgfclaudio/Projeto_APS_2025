### UC.8 - Enviar Documentação
```mermaid
classDiagram
    class EnvioDocForm {
        <<boundary>>
        +selecionarArquivos()
    }

    class DocumentacaoController {
        <<control>>
        +validarDocumentos()
        +anexarDocumentos()
    }

    class Documento {
        <<entity>>
        +tipo: String
        +arquivo: File
        +atividadeVinculada: String
    }

    class DocumentoCollection {
        <<entity collection>>
        +anexar(Documento)
        +verificarObrigatoriedade()
    }

    EnvioDocForm --> DocumentacaoController : submete arquivos
    DocumentacaoController --> Documento : cria instâncias
    DocumentacaoController --> DocumentoCollection : armazena
