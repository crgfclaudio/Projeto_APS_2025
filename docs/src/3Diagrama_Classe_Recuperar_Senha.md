### UC.3 - Recuperar Senha
```mermaid
classDiagram
    class RecuperacaoForm {
        <<boundary>>
        +email: String
    }

    class RecuperacaoController {
        <<control>>
        +validarEmail()
        +enviarLink()
    }

    class Usuario {
        <<entity>>
        +email: String
        +senha: String
    }

    class UsuarioCollection {
        <<entity collection>>
        +buscarPorEmail(email)
    }

    RecuperacaoForm --> RecuperacaoController : submete
    RecuperacaoController --> UsuarioCollection : busca usuário
    UsuarioCollection --> Usuario : verifica existência
    RecuperacaoController --> RecuperacaoForm : envia feedback
