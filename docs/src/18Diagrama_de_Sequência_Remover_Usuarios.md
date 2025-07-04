### UC.18 – Remover Usuário (Administrador)
```mermaid
sequenceDiagram
    actor Administrador

    participant PainelAdministrador      as PainelAdministrador (Boundary)
    participant UsuarioAdminController  as UsuarioAdminController (Control)
    participant UsuarioCollection       as UsuarioCollection (EntityCollection)
    participant Usuario                 as Usuario (Entity)

    Administrador->>PainelAdministrador: visualizarUsuarios()
    PainelAdministrador->>UsuarioAdminController: fetchUsuarios()
    UsuarioAdminController->>UsuarioCollection: buscarTodos()
    UsuarioCollection-->>UsuarioAdminController: listaUsuarios
    UsuarioAdminController-->>PainelAdministrador: exibirUsuarios(listaUsuarios)

    Administrador->>PainelAdministrador: removerUsuario(idUsuario)
    PainelAdministrador->>UsuarioAdminController: removerUsuario(idUsuario)
    UsuarioAdminController->>UsuarioAdminController: verificarVinculos(idUsuario)
    alt possui vínculos
        UsuarioAdminController-->>PainelAdministrador: mostrarErro("Usuário possui vínculos")
    else sem vínculos
        UsuarioAdminController->>UsuarioCollection: buscarPorId(idUsuario)
        UsuarioCollection-->>UsuarioAdminController: usuario
        UsuarioAdminController->>UsuarioCollection: remover(usuario)
        UsuarioCollection-->>UsuarioAdminController: confirmacaoRemocao
        UsuarioAdminController-->>PainelAdministrador: mostrarConfirmacao("Usuário removido")
    end
