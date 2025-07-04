### UC.13 - Encerrar Sessão
```mermaid
classDiagram
    class MenuUsuario {
        <<boundary>>
        +encerrarSessao()
    }

    class SessaoController {
        <<control>>
        +finalizarSessao()
    }

    class Sessao {
        <<entity>>
        +idUsuario: String
        +inicio: DateTime
        +fim: DateTime
    }

    MenuUsuario --> SessaoController : requisita logout
    SessaoController --> Sessao : encerra
