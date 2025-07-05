### UC.5 - Submeter Relatório Final
```mermaid
sequenceDiagram
    actor Aluno
    participant EnvioRelatorioFinalForm as EnvioRelatorioFinalForm (Boundary)
    participant SubmissaoFinalController as SubmissaoFinalController (Control)
    participant RelatorioCollection as RelatorioCollection (EntityCollection)
    participant DB as PostgreSQL

    %% 1. Envio do formulário
    Aluno->>EnvioRelatorioFinalForm: anexarArquivo(PDF)
    EnvioRelatorioFinalForm->>SubmissaoFinalController: POST /relatorioFinal (dados, arquivo)

    %% 2. Verificação de submissão existente
    SubmissaoFinalController->>RelatorioCollection: verificarSubmissaoFinal(idAluno)
    RelatorioCollection->>DB: SELECT * FROM relatorios WHERE aluno_id = ? AND tipo = 'final'

    alt Submissão já realizada
        DB-->>RelatorioCollection: relatório existente
        RelatorioCollection-->>SubmissaoFinalController: duplicado
        SubmissaoFinalController-->>EnvioRelatorioFinalForm: erro("Submissão já realizada")
        EnvioRelatorioFinalForm-->>Aluno: exibir erro
    else Submissão permitida
        DB-->>RelatorioCollection: nenhum encontrado
        RelatorioCollection-->>SubmissaoFinalController: permitido

        %% 3. Criação e inserção do relatório
        SubmissaoFinalController->>RelatorioCollection: adicionarRelatorioFinal(idAluno, dados, arquivo)
        RelatorioCollection->>DB: INSERT INTO relatorios (...)
        DB-->>RelatorioCollection: OK
        RelatorioCollection-->>SubmissaoFinalController: confirmação
        SubmissaoFinalController-->>EnvioRelatorioFinalForm: sucesso("Relatório submetido")
        EnvioRelatorioFinalForm-->>Aluno: exibir confirmação
    end
