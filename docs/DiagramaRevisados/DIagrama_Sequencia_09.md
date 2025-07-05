### UC.9 - Se Candidatar à Monitoria
```mermaid
sequenceDiagram
    actor Aluno
    participant CandidaturaForm as CandidaturaForm (Boundary)
    participant MonitoriaController as MonitoriaController (Control)
    participant CandidaturaCollection as CandidaturaCollection (EntityCollection)
    participant DB as PostgreSQL

    %% 1. Entrada do aluno
    Aluno->>CandidaturaForm: preencherDados()
    CandidaturaForm->>CandidaturaForm: anexarDocs(files)
    CandidaturaForm->>MonitoriaController: POST /candidatura (dados, files)

    %% 2. Validação dos requisitos
    MonitoriaController->>MonitoriaController: validarRequisitos(dados)
    alt Requisitos não atendidos
        MonitoriaController-->>CandidaturaForm: mostrarErro("Requisitos não atendidos")
    else Requisitos atendidos
        %% 3. Verificação de duplicidade
        MonitoriaController->>CandidaturaCollection: verificarDuplicidade(idAluno, idMonitoria)
        CandidaturaCollection->>DB: SELECT * FROM candidaturas WHERE aluno_id = ? AND monitoria_id = ?
        alt Candidatura já existente
            DB-->>CandidaturaCollection: resultado encontrado
            CandidaturaCollection-->>MonitoriaController: duplicada
            MonitoriaController-->>CandidaturaForm: mostrarErro("Candidatura já enviada")
        else Candidatura nova
            DB-->>CandidaturaCollection: nenhum resultado
            CandidaturaCollection-->>MonitoriaController: não duplicada

            %% 4. Registro da candidatura
            MonitoriaController->>CandidaturaCollection: adicionar(dados, files)
            CandidaturaCollection->>DB: INSERT INTO candidaturas (...)
            DB-->>CandidaturaCollection: sucesso
            CandidaturaCollection-->>MonitoriaController: confirmação
            MonitoriaController-->>CandidaturaForm: mostrarSuccess("Candidatura registrada com sucesso")
        end
    end

