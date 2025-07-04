### UC.2 - Registrar Usuário
```mermaid
classDiagram
    class FormCadastro {
        <<boundary>>
        +tipo: String
        +dados: CSV | Formulário
    }

    class CadastroController {
        <<control>>
        +validarDados()
        +registrarUsuario()
    }

    class Usuario {
        <<entity>>
        +nome: String
        +cpf: String
        +email: String
        +perfil: String
    }

    class UsuarioCollection {
        <<entity collection>>
        +adicionar(Usuario)
        +verificarDuplicidade()
    }

    FormCadastro --> CadastroController : envia dados
    CadastroController --> Usuario : cria instância
    CadastroController --> UsuarioCollection : salva
    UsuarioCollection --> CadastroController : valida

