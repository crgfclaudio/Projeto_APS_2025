'''Mermaid
sequenceDiagram
    actor Aluno

    participant EstagioForm          as EstagioForm (Boundary)
    participant EstagioController    as EstagioController (Control)
    participant Estagio              as Estagio (Entity)
    participant EstagioCollection    as EstagioCollection (EntityCollection)

    Aluno->>EstagioForm: preencherFormulario(dados)
    EstagioForm->>EstagioForm: anexarDocumento(file)
    EstagioForm->>EstagioController: registrarSolicitacao(dados, file)

    EstagioController->>EstagioController: validarDados(dados, file)
    alt dadosInválidos
        EstagioController-->>EstagioForm: mostrarErro("Dados inválidos")
    else dadosVálidos
        EstagioController->>Estagio: new Estagio(dados)
        EstagioController->>EstagioCollection: validarRegras(estagio)
        EstagioCollection-->>EstagioController: regrasValidadas?
        alt regrasInválidas
            EstagioController-->>EstagioForm: mostrarErro("Regras não atendidas")
        else regrasVálidas
            EstagioController->>EstagioCollection: adicionar(estagio)
            EstagioCollection-->>EstagioController: confirmação
            EstagioController-->>EstagioForm: mostrarSuccess("Estágio registrado")
        end
    end
