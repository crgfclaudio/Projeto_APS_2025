### UC.30 - Atribuir Nota

```mermaid
%% Diagrama de Classe para UC.30 - Atribuir Nota
classDiagram
    class PainelProfessor {
        <<boundary>>
        +acessarAcompanhamento()
        +visualizarRelatorios()
        +avaliarRelatorio()
    }

    class AvaliacaoController {
        <<control>>
        +validarEntrada()
        +registrarAvaliacao()
    }

    class Relatorio {
        <<entity>>
        +conteudo: String
        +dataEnvio: Date
        +avaliacao: Avaliacao
    }

    class Avaliacao {
        <<entity>>
        +nota: Float
        +comentario: String
        +dataAvaliacao: Date
    }

    class NotificacaoService {
        <<service>>
        +notificarAluno()
    }

    PainelProfessor --> AvaliacaoController : envia dados de avaliação
    AvaliacaoController --> Relatorio : atualiza avaliação
    AvaliacaoController --> NotificacaoService : notifica aluno

    PainelProfessor --> LiberacaoTCCController : solicita liberação
    LiberacaoTCCController --> TCCCollection : atualiza status
