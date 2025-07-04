### UC.2 - Registrar Usuário
```mermaid
classDiagram
    class Administrador {
        +registrarUsuario(dados)
    }

    class Usuario {
        +String nome
        +String email
        +String cpf
        +String perfil
    }

    Administrador --> Usuario : cria
