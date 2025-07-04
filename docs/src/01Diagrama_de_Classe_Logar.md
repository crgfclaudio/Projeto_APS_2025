### UC.1 - Logar
```mermaid
classDiagram
    class LoginForm {
        <<boundary>>
        +cpf: String
        +senha: String
    }

    class LoginController {
        <<control>>
        +autenticar(cpf, senha)
    }

    class Usuario {
        <<entity>>
        +cpf: String
        +senha: String
        +perfil: String
    }

    class UsuarioCollection {
        <<entity collection>>
        +usuarios: List<Usuario>
        +buscarUsuario(cpf)
    }

    LoginForm --> LoginController : envia dados
    LoginController --> UsuarioCollection : consulta
    UsuarioCollection --> Usuario : retorna dados
    LoginController --> LoginForm : autenticação ok/erro
