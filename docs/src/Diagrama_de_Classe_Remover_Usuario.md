### UC.18 - Remover Usuário(Administrador)
```mermaid
classDiagram
    class PainelAdministrador {
        <<boundary>>
        +visualizarUsuarios()
        +removerUsuario()
    }

    class UsuarioAdminController {
        <<control>>
        +verificarVinculos()
        +removerUsuario()
    }

    class Usuario {
        <<entity>>
        +id: String
        +nome: String
        +ativo: Boolean
    }

    class UsuarioCollection {
        <<entity collection>>
        +buscarPorId()
        +remover()
    }

    PainelAdministrador --> UsuarioAdminController : inicia remoção
    UsuarioAdminController --> UsuarioCollection : consulta e remove
