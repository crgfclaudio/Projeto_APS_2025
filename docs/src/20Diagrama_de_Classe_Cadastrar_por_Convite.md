### UC.20 - Cadastrar por Convite(Administrador)
```mermaid
classDiagram
    class PainelAdministrador {
        <<boundary>>
        +inserirEmailProfessor()
        +enviarConvite()
    }

    class ConviteAdminController {
        <<control>>
        +validarEmail()
        +gerarLinkConvite()
        +enviarEmail()
    }

    class Usuario {
        <<entity>>
        +email: String
        +linkCadastro: String
    }

    class UsuarioCollection {
        <<entity collection>>
        +verificarEmail()
        +registrarConvite()
    }

    PainelAdministrador --> ConviteAdminController : envia dados
    ConviteAdminController --> UsuarioCollection : registra convite
    UsuarioCollection --> Usuario : gera link
