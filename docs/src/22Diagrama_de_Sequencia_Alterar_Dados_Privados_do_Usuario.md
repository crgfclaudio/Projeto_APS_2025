### UC.22 - Alterar Dados Privados de Usuário (Administrador)
```mermaid
sequenceDiagram
    actor Administrador
    participant PainelAdministrador as UI: PainelAdministrador (React)
    participant EdicaoPrivadaController as Controller: EdicaoPrivadaController (Express/Django)
    participant UsuarioCollection as Repository: UsuarioCollection
    participant Usuario as Entity: Usuario
    participant DB as PostgreSQL

    %% Acesso ao usuário
    Administrador->>PainelAdministrador: buscar usuário por ID
    PainelAdministrador->>EdicaoPrivadaController: GET /usuario/:id
    EdicaoPrivadaController->>UsuarioCollection: buscarPorId(id)
    UsuarioCollection->>DB: SELECT * FROM usuarios WHERE id = ?
    DB-->>UsuarioCollection: dados do usuário
    UsuarioCollection-->>EdicaoPrivadaController: retornar usuário
    EdicaoPrivadaController-->>PainelAdministrador: preencherForm(dados)

    %% Alteração dos dados
    Administrador->>PainelAdministrador: editarDadosSensiveis()
    PainelAdministrador->>EdicaoPrivadaController: PUT /usuario/:id (novos dados)
    EdicaoPrivadaController->>EdicaoPrivadaController: validarCampos(dados)

    alt Dados inválidos
        EdicaoPrivadaController-->>PainelAdministrador: exibirErro("Campos inválidos")
    else Dados válidos
        EdicaoPrivadaController->>UsuarioCollection: atualizarUsuario(id, novosDados)
        UsuarioCollection->>DB: UPDATE usuarios SET cpf=..., email=..., matricula=... WHERE id = ?
        DB-->>UsuarioCollection: OK
        UsuarioCollection-->>EdicaoPrivadaController: confirmação
        EdicaoPrivadaController-->>PainelAdministrador: exibirSucesso("Dados atualizados com sucesso")
    end

