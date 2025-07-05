### UC.18 – Remover Usuário (Administrador)
```mermaid
sequenceDiagram
    actor Administrador
    participant PainelAdministrador as PainelAdministrador (Boundary)
    participant UsuarioAdminController as UsuarioAdminController (Control)
    participant UsuarioCollection as UsuarioCollection (EntityCollection)
    participant Usuario as Usuario (Entity)
    participant DB as PostgreSQL

    %% 1. Visualização inicial dos usuários
    Administrador->>PainelAdministrador: visualizarUsuarios()
    PainelAdministrador->>UsuarioAdminController: fetchUsuarios()
    UsuarioAdminController->>UsuarioCollection: buscarTodos()
    UsuarioCollection->>DB: SELECT * FROM usuarios
    DB-->>UsuarioCollection: lista de usuários
    UsuarioCollection-->>UsuarioAdminController: listaUsuarios
    UsuarioAdminController-->>PainelAdministrador: exibirUsuarios(listaUsuarios)

    %% 2. Requisição de remoção
    Administrador->>PainelAdministrador: removerUsuario(id)
    PainelAdministrador->>UsuarioAdminController: removerUsuario(id)
    UsuarioAdminController->>UsuarioCollection: verificarVinculos(id)
    UsuarioCollection->>DB: SELECT * FROM vinculos WHERE usuario_id = ?
    alt Possui vínculos
        DB-->>UsuarioCollection: resultado encontrado
        UsuarioCollection-->>UsuarioAdminController: possuiVinculos = true
        UsuarioAdminController-->>PainelAdministrador: mostrarErro("Usuário possui vínculos")
    else Sem vínculos
        DB-->>UsuarioCollection: nenhum resultado
        UsuarioCollection-->>UsuarioAdminController: possuiVinculos = false
        UsuarioAdminController->>UsuarioCollection: buscarPorId(id)
        UsuarioCollection->>DB: SELECT * FROM usuarios WHERE id = ?
        DB-->>UsuarioCollection: dados do usuário
        UsuarioCollection-->>UsuarioAdminController: usuario
        UsuarioAdminController->>UsuarioCollection: remover(usuario)
        UsuarioCollection->>DB: DELETE FROM usuarios WHERE id = ?
        DB-->>UsuarioCollection: OK
        UsuarioCollection-->>UsuarioAdminController: confirmacaoRemocao
        UsuarioAdminController-->>PainelAdministrador: mostrarConfirmacao("Usuário removido com sucesso")
    end
