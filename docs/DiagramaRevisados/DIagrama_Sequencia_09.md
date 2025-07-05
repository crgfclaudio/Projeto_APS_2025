### UC.9 - Se Candidatar à Monitoria com arquitetura
```mermaid
sequenceDiagram
    participant WebApp as WebApp (Apresentação)
    participant Auth as AuthService
    participant Aluno as AlunoService
    participant Modulos as Gerenciador de Módulos
    participant Relatorio as Módulo Relatório
    participant DB as DatabaseService
    participant MQ as RabbitMQ
    participant Email as Sistema de e-mail

    WebApp->>Auth: autenticar(login/sessão)
    Auth-->>WebApp: token de sessão válido

    WebApp->>Aluno: enviarRelatorio(token, dados PDF)
    Aluno->>Modulos: encaminharParaRelatorio(dados)
    Modulos->>Relatorio: processarSubmissaoRelatorio(dados PDF)

    Relatorio->>DB: salvarRelatorio(dados, metadata)
    DB-->>Relatorio: confirmação de armazenamento

    Relatorio->>MQ: publicarNotificacao(professorID, tipo: nova_submissao)
    MQ-->>Email: enviarEmail(professor@email)

    Relatorio-->>WebApp: status = "submissão recebida"

            CandidaturaCollection-->>MonitoriaController: confirmação
            MonitoriaController-->>CandidaturaForm: mostrarSuccess("Candidatura registrada com sucesso")
        end
    end

