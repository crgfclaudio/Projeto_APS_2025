### UC.5 - Submeter Relatório Final com arquitetura
```mermaid
sequenceDiagram
    actor Aluno

    participant WebApp as WebApp (Apresentação)
    participant AlunoService as AlunoService (Controller)
    participant Modulos as Gerenciador de Módulos
    participant Relatorio as Módulo Relatório
    participant DB as DatabaseService
    participant DocDB as Doc DB

    %% 1. Submissão via WebApp
    Aluno->>WebApp: enviarRelatorioFinal(PDF, metadados)
    WebApp->>AlunoService: POST /relatorio/final (dados, arquivo)

    %% 2. Direcionamento do fluxo
    AlunoService->>Modulos: encaminharParaModulo("relatorioFinal")
    Modulos->>Relatorio: processarSubmissaoFinal(idAluno, dados, arquivo)

    %% 3. Verificação de submissão duplicada
    Relatorio->>DB: verificarSubmissaoFinal(idAluno)
    DB->>DocDB: SELECT * FROM relatorios WHERE aluno_id = ? AND tipo = 'final'

    alt Submissão já realizada
        DocDB-->>DB: relatório existente
        DB-->>Relatorio: duplicado
        Relatorio-->>Modulos: erro("Relatório final já submetido")
        Modulos-->>AlunoService: erro duplicidade
        AlunoService-->>WebApp: exibirErro("Submissão já realizada")
        WebApp-->>Aluno: mostrarErro
    else Submissão permitida
        DocDB-->>DB: nenhum encontrado
        DB-->>Relatorio: ok

        %% 4. Inserção no banco
        Relatorio->>DB: salvarRelatorioFinal(idAluno, dados, arquivo)
        DB->>DocDB: INSERT INTO relatorios (tipo='final', ...)
        DocDB-->>DB: OK
        DB-->>Relatorio: confirmação

        %% 5. Confirmação
        Relatorio-->>Modulos: sucesso
        Modulos-->>AlunoService: relatório final salvo
        AlunoService-->>WebApp: exibirSucesso("Relatório final submetido com sucesso")
        WebApp-->>Aluno: mostrarConfirmação
    end
