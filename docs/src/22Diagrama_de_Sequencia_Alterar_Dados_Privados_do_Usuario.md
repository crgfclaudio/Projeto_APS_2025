### UC.22 - Alterar Dados Privados de Usuário (Administrador)
```mermaid
sequenceDiagram
    actor Aluno

    participant CandidaturaForm        as CandidaturaForm (Boundary)
    participant MonitoriaController    as MonitoriaController (Control)
    participant CandidaturaCollection  as CandidaturaCollection (EntityCollection)
    participant Candidatura            as Candidatura (Entity)

    Aluno->>CandidaturaForm: preencherDados()
    CandidaturaForm->>CandidaturaForm: anexarDocs(files)
    CandidaturaForm->>MonitoriaController: registrarCandidatura(dados, files)

    MonitoriaController->>MonitoriaController: validarRequisitos(dados)
    alt requisitos não atendidos
        MonitoriaController-->>CandidaturaForm: mostrarErro("Requisitos não atendidos")
    else requisitos atendidos
        MonitoriaController->>CandidaturaCollection: verificarDuplicidade(idAluno, idMonitoria)
        CandidaturaCollection-->>MonitoriaController: duplicado?
        alt duplicada
            MonitoriaController-->>CandidaturaForm: mostrarErro("Candidatura já existe")
        else não duplicada
            MonitoriaController->>Candidatura: new Candidatura(dados, files)
            MonitoriaController->>CandidaturaCollection: adicionar(candidatura)
            CandidaturaCollection-->>MonitoriaController: confirmação
            MonitoriaController-->>CandidaturaForm: mostrarSuccess("Candidatura enviada")
        end
    end
