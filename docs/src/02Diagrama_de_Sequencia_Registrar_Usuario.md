### UC.2 - Registrar Usuário
```mermaid
sequenceDiagram
    
    participant FormCadastro
    participant CadastroController
    participant Usuario
    participant UsuarioCollection

    FormCadastro->>CadastroController: enviarDados(dados)
    CadastroController->>UsuarioCollection: verificarDuplicidade(dados)
    UsuarioCollection-->>CadastroController: resultadoVerificação
    
    alt Dados válidos e não duplicados
        CadastroController->>Usuario: criarInstância(dados)
        CadastroController->>UsuarioCollection: adicionar(usuario)
        UsuarioCollection-->>CadastroController: confirmação
        CadastroController-->>FormCadastro: sucesso
    
    else Dados inválidos ou duplicados
        CadastroController-->>FormCadastro: erro
    
    end

