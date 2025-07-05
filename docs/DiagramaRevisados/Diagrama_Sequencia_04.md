### UC.4 - Submeter Relatório Parcial
```mermaid
sequenceDiagram
    actor Aluno
    participant EnvioRelatorioForm as EnvioRelatorioForm (Boundary)
    participant SubmissaoController as SubmissaoController (Control)
    participant RelatorioCollection as RelatorioCollection (EntityCollection)
    participant DB as PostgreSQL

    %% 1. Submissão pelo formulário
    Aluno->>EnvioRelatorioForm: anexarArquivo(PDF)
    EnvioRelatorioForm->>SubmissaoController: POST /relatorioParcial (dados, arquivo)

    %% 2. Criação e validação
    SubmissaoController->>RelatorioCollection: verificarDuplicidade(idAluno, tipo="parcial")
    RelatorioCollection->>DB: SELECT * FROM relatorios WHERE aluno_id = ? AND tipo = 'parcial'

    alt Já existe relatório parcial
        DB-->>RelatorioCollection: duplicado
        RelatorioCollection-->>SubmissaoController: duplicado
        SubmissaoController-->>EnvioRelatorioForm: erro("Relatório parcial já submetido")
        EnvioRelatorioForm-->>Aluno: mostrar erro
    else Submissão permitida
        DB-->>RelatorioCollection: nenhum encontrado
        RelatorioCollection-->>SubmissaoController: não duplicado

        %% 3. Registro no banco
        SubmissaoController->>RelatorioCollection: adicionarRelatorioParcial(dados, arquivo)
        RelatorioCollection->>DB: INSERT INTO relatorios (...)
        DB-->>RelatorioCollection: OK
        RelatorioCollection-->>SubmissaoController: confirmação
        SubmissaoController-->>EnvioRelatorioForm: sucesso("Relatório parcial submetido")
        EnvioRelatorioForm-->>Aluno: mostrar confirmação
    end

