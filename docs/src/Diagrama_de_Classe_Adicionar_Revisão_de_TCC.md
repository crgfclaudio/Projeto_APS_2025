### UC.29 - Adicionar Revisão de TCC (Professor)

```mermaid
%% Diagrama de Classe para UC.29 - Adicionar Revisão de TCC
classDiagram
    class PainelProfessor {
        <<boundary>>
        +visualizarTCC()
        +adicionarRevisao()
    }

    class TCCController {
        <<control>>
        +registrarRevisao()
        +validarVersao()
    }

    class TCC {
        <<entity>>
        +titulo: String
        +aluno: String
        +versaoAtual: int
    }

    class Revisao {
        <<entity>>
        +comentarios: String
        +data: Date
        +versao: int
    }

    class RevisaoCollection {
        <<entity collection>>
        +adicionar()
        +listarPorTCC()
    }

    PainelProfessor --> TCCController : envia revisão
    TCCController --> Revisao : instancia
    TCCController --> RevisaoCollection : armazena
