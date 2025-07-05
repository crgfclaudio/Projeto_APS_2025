### UC.22 - Alterar Dados Privados de Usuário (Administrador) com Arquitetura
```mermaid
sequenceDiagram
    actor Administrador

    participant WebApp as WebApp (Apresentação)
    participant ADMService as ADMService (Controller)
    participant DB as DatabaseService
    participant DocDB as Doc DB

    %% 1. Acesso aos dados do usuário
    Administrador->>WebApp: buscarUsuarioPorId(id)
    WebApp->>ADMService: GET /usuarios/{id}
    ADMService->>DB: consultarUsuarioPorId(id)
    DB->>DocDB: SELECT * FROM usuarios WHERE id = ?
    DocDB-->>DB: dados do usuário
    DB-->>ADMService: retornoUsuario
    ADMService-->>WebApp: preencherForm(dados)
    WebApp-->>Administrador: exibirDadosEmForm

    %% 2. Requisição de edição
    Administrador->>WebApp: editarDadosPrivados()
    WebApp->>ADMService: PUT /usuarios/{id} (dados sensíveis)
    ADMService->>ADMService: validarCampos(dados)

    alt Dados inválidos
        ADMService-->>WebApp: erro("Campos inválidos")
        WebApp-->>Administrador: mostrarErro
    else Dados válidos
        ADMService->>DB: atualizarUsuarioPrivado(id, dados)
        DB->>DocDB: UPDATE usuarios SET cpf=..., email=..., matricula=... WHERE id = ?
        DocDB-->>DB: OK
        DB-->>ADMService: confirmação
        ADMService-->>WebApp: sucesso("Dados atualizados com sucesso")
        WebApp-->>Administrador: exibirConfirmacao
    end

