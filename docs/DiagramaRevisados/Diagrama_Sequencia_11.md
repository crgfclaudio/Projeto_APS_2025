### UC.11 - Registrar Estágio
```mermaid
sequenceDiagram
    actor Aluno
    participant EstagioForm as EstagioForm (Boundary)
    participant EstagioController as EstagioController (Control)
    participant EstagioCollection as EstagioCollection (EntityCollection)
    participant Estagio as Estagio (Entity)
    participant DB as PostgreSQL

    %% 1. Entrada dos dados pelo formulário
    Aluno->>EstagioForm: preencherFormulario(dados)
    EstagioForm->>EstagioForm: anexarDocumento(file)
    EstagioForm->>EstagioController: registrarSolicitacao(dados, file)

    %% 2. Validação dos dados de entrada
    EstagioController->>EstagioController: validarDados(dados, file)
    alt Dados inválidos
        EstagioController-->>EstagioForm: mostrarErro("Dados inválidos")
    else Dados válidos
        %% 3. Instancia o objeto Estágio
        EstagioController->>Estagio: criarInstancia(dados)

        %% 4. Verifica regras de negócio (como período, carga horária, etc.)
        EstagioController->>EstagioCollection: validarRegras(estagio)
        EstagioCollection-->>EstagioController: resultado das regras

        alt Regras não atendidas
            EstagioCont
