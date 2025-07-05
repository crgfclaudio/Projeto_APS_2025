### UC.18 – Remover Usuário (Administrador) com Arquitetura
```mermaid
sequenceDiagram
    actor Administrador

    participant WebApp as WebApp (Apresentação)
    participant ADMService as ADMService (Controller)
    participant DB as DatabaseService
    participant DocDB as Doc DB

    %% 1. Listagem inicial de usuários
    Administrador->>WebApp: visualizarUsuarios()
    WebApp->>ADMService: GET /usuarios
    ADMService->>DB: consultarTodosUsuarios()
    DB->>DocDB: SELECT * FROM usuarios
    DocDB-->>DB: lista de usuários
    DB-->>ADMService: retornoUsuarios
    ADMService-->>WebApp: exibirUsuarios(lista)
    WebApp-->>Administrador: exibirListaNaTela

    %% 2. Solicitação de remoção
    Administrador->>WebApp: removerUsuario(id)
    WebApp->>ADMService: DELETE /usuarios/{id}

    %% 3. Verificação de vínculos
    ADMService->>DB: verificarVinculosUsuario(id)
    DB->>DocDB: SELECT * FROM vinculos WHERE usuario_id = ?
    alt Usuário possui vínculos
        DocDB-->>DB: vínculos encontrados
        DB-->>ADMService: possuiVinculos = true
        ADMService-->>WebApp: erro("Usuário possui vínculos e não pode ser removido")
        WebApp-->>Administrador: exibirErro
    else Sem vínculos
        DocDB-->>DB: nenhum vínculo encontrado
        DB-->>ADMService: possuiVinculos = false

        %% 4. Remoção segura
        ADMService->>DB: buscarUsuarioPorId(id)
        DB->>DocDB: SELECT * FROM usuarios WHERE id = ?
        DocDB-->>DB: dados do usuário
        DB-->>ADMService: dadosUsuario

        ADMService->>DB: excluirUsuario(id)
        DB->>DocDB: DELETE FROM usuarios WHERE id = ?
        DocDB-->>DB: OK
        DB-->>ADMService: confirmacaoRemocao

        %% 5. Confirmação
        ADMService-->>WebApp: mostrarConfirmacao("Usuário removido com sucesso")
        WebApp-->>Administrador: exibirConfirmação
    end

