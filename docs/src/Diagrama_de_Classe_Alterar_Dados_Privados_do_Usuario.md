### UC.22 - Alterar Dados Privados de Usuário (Administrador)

```mermaid
%% Diagrama de Classe para UC.22 - Alterar Dados Privados
classDiagram
    class PainelAdministrador {
        <<boundary>>
        +buscarUsuario()
        +editarDadosSensiveis()
    }

    class EdicaoPrivadaController {
        <<control>>
        +validarCampos()
        +salvarAlteracoes()
    }

    class Usuario {
        <<entity>>
        +cpf: String
        +emailInstitucional: String
        +matricula: String
    }

    class UsuarioCollection {
        <<entity collection>>
        +buscarPorId()
        +atualizarUsuario()
    }

    PainelAdministrador --> EdicaoPrivadaController : edita dados
    EdicaoPrivadaController --> UsuarioCollection : atualiza
