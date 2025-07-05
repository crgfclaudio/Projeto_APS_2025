### UC.24 - Aceitar Solicitação de Estágio (Professor) com Arquitetura

````mermaid
sequenceDiagram
    actor Professor

    participant WebApp as WebApp (Apresentação)
    participant ProfessorService as ProfessorService (Controller)
    participant Modulos as Gerenciador de Módulos
    participant Estagio as Módulo Estágio
    participant DB as DatabaseService
    participant DocDB as Doc DB

    %% Fluxo de visualização da solicitação
    Professor->>WebApp: visualizarSolicitacao(id)
    WebApp->>ProfessorService: GET /solicitacoes/{id}
    ProfessorService->>Modulos: buscarSolicitacao(id)
    Modulos->>Estagio: obterDetalhesSolicitacao(id)
    Estagio->>DB: consultarSolicitacao(id)
    DB->>DocDB: SELECT * FROM solicitacoes WHERE id = ?
    DocDB-->>DB: dados da solicitação
    DB-->>Estagio: dados recebidos
    Estagio-->>Modulos: detalhes da solicitação
    Modulos-->>ProfessorService: retorno com dados
    ProfessorService-->>WebApp: exibirDetalhes(solicitacao)
    WebApp-->>Professor: exibir detalhes

    %% Fluxo principal – Decisão do professor
    alt Aprovar
        Professor->>WebApp: aceitarSolicitacao(id)
        WebApp->>ProfessorService: POST /solicitacoes/{id}/aprovar
        ProfessorService->>Modulos: aprovarSolicitacao(id)
        Modulos->>Estagio: validarSolicitacao(id)

        alt Solicitação válida
            Estagio->>DB: atualizarStatus(id, "Aprovada")
            DB->>DocDB: UPDATE solicitacoes SET status = 'Aprovada' WHERE id = ?
            DocDB-->>DB: OK
            DB-->>Estagio: sucesso
            Estagio-->>Modulos: confirmação
            Modulos-->>ProfessorService: sucesso
            ProfessorService-->>WebApp: mostrarConfirmacao("Solicitação aprovada")
            WebApp-->>Professor: exibir sucesso
        else Solicitação inválida
            Estagio-->>Modulos: erro("Solicitação inválida")
            Modulos-->>ProfessorService: erro
            ProfessorService-->>WebApp: mostrarErro("Solicitação inválida")
            WebApp-->>Professor: exibir erro
        end
    else Rejeitar
        Professor->>WebApp: rejeitarSolicitacao(id)
        WebApp->>ProfessorService: POST /solicitacoes/{id}/rejeitar
        ProfessorService->>Modulos: rejeitarSolicitacao(id)
        Modulos->>Estagio: validarSolicitacao(id)

        alt Solicitação válida
            Estagio->>DB: atualizarStatus(id, "Rejeitada")
            DB->>DocDB: UPDATE solicitacoes SET status = 'Rejeitada' WHERE id = ?
            DocDB-->>DB: OK
            DB-->>Estagio: sucesso
            Estagio-->>Modulos: confirmação
            Modulos-->>ProfessorService: sucesso
            ProfessorService-->>WebApp: mostrarConfirmacao("Solicitação rejeitada")
            WebApp-->>Professor: exibir sucesso
        else Solicitação inválida
            Estagio-->>Modulos: erro("Solicitação inválida")
            Modulos-->>ProfessorService: erro
            ProfessorService-->>WebApp: mostrarErro("Solicitação inválida")
            WebApp-->>Professor: exibir erro
        end
    end

