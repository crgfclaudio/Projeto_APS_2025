### UC.30 - Liberar para Publicação de TCC (Professor)

```mermaid
%% Diagrama de Classe para UC.30 - Liberar para Publicação de TCC
classDiagram
    class PainelProfessor {
        <<boundary>>
        +visualizarTCC()
        +liberarParaPublicacao()
    }

    class LiberacaoTCCController {
        <<control>>
        +validarVersaoFinal()
        +liberar()
    }

    class TCC {
        <<entity>>
        +titulo: String
        +versaoFinal: File
        +status: String
        +aprovadoPorProfessor: Boolean
    }

    class TCCCollection {
        <<entity collection>>
        +buscar()
        +atualizarStatus()
    }

    PainelProfessor --> LiberacaoTCCController : solicita liberação
    LiberacaoTCCController --> TCCCollection : atualiza status
