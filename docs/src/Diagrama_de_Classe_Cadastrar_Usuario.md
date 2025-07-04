### UC.19 - Cadastrar Usuário(Administrador)
classDiagram
    class PainelAdministrador {
        <<boundary>>
        +acessarCadastroManual()
        +submeterDados()
    }

    class CadastroAdminController {
        <<control>>
        +validarDados()
        +cadastrarUsuario()
    }

    class Usuario {
        <<entity>>
        +nome: String
        +email: String
        +cpf: String
        +perfil: String
    }

    class UsuarioCollection {
        <<entity collection>>
        +verificarDuplicidade()
        +adicionar()
    }

    PainelAdministrador --> CadastroAdminController : envia dados
    CadastroAdminController --> Usuario : instancia
    CadastroAdminController --> UsuarioCollection : armazena
