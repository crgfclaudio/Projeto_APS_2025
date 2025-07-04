### UC.17 - Alterar Dados (Usuário Logado)

```mermaid
%% Diagrama de Classe para UC.17 - Alterar Dados (Usuário Logado)
classDiagram
    class PerfilForm {
        <<boundary>>
        +editarNome()
        +editarTelefone()
        +editarEndereco()
    }

    class PerfilController {
        <<control>>
        +validarDados()
        +atualizarUsuario()
    }

    class Usuario {
        <<entity>>
        +cpf: String
        +nome: String
        +telefone: String
        +endereco: String
        +email: String
    }

    class UsuarioCollection {
        <<entity collection>>
        +buscarUsuario()
        +salvarAlteracoes()
    }

    PerfilForm --> PerfilController : envia alterações
    PerfilController --> UsuarioCollection : atualiza
    UsuarioCollection --> Usuario : persiste mudanças
