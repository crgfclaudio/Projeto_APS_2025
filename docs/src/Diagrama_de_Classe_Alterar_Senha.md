### UC.14 - Alterar Senha
```mermaid
classDiagram
    class AlterarSenhaForm {
        <<boundary>>
        +senhaAtual: String
        +novaSenha: String
    }

    class SenhaController {
        <<control>>
        +validarAtual()
        +atualizarSenha()
    }

    class Usuario {
        <<entity>>
        +cpf: String
        +senha: String
    }

    class UsuarioCollection {
        <<entity collection>>
        +buscarPorCPF()
        +salvarNovaSenha()
    }

    AlterarSenhaForm --> SenhaController : envia dados
    SenhaController --> UsuarioCollection : consulta
    UsuarioCollection --> Usuario : atualiza senha
