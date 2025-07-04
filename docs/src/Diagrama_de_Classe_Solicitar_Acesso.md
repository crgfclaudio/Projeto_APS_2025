### UC.12 - Solicitar Acesso
```mermaid
classDiagram
    class AcessoForm {
        <<boundary>>
        +inserirCPF()
        +definirNovaSenha()
    }

    class AcessoController {
        <<control>>
        +verificarCadastro()
        +registrarNovaSenha()
    }

    class Usuario {
        <<entity>>
        +cpf: String
        +senha: String
    }

    class UsuarioCollection {
        <<entity collection>>
        +buscarPorCPF(cpf)
        +atualizarSenha()
    }

    AcessoForm --> AcessoController : solicita criação
    AcessoController --> UsuarioCollection : busca/atualiza
    UsuarioCollection --> Usuario : modifica senha
