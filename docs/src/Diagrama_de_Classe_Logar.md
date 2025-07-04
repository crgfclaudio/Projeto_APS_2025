### UC.1 - Logar
```mermaid
classDiagram
    class Usuario {
        +String cpf
        +String senha
        +boolean autenticado
    }

    class Sistema {
        +autenticarUsuario(cpf, senha)
    }

    Usuario --> Sistema : envia credenciais
    Sistema --> Usuario : autentica acesso

