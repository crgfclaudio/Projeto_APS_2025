### UC.16 - Aprovar Solicitação (Professor) com Arquitetura
```mermaid
sequenceDiagram
    actor Professor

    participant WebApp as WebApp (Apresentação)
    participant ProfessorService as ProfessorService (Controller)
    participant Modulos as Gerenciador de Módulos
    participant Estagio as Módulo Estágio
    participant DB as DatabaseService
    participant DocDB as Doc DB

    %% Visualização inicial da solicitação
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
    WebApp-->>Professor: detalhes da solicitação

    %% Decisão do professor
    alt Aprovar
        Professor->>WebApp: aprovarSolicitacao(id)
        WebApp->>ProfessorService: POST /solicitacoes/{id}/aprovar
        ProfessorService->>Modulos: aprovarSolicitacao(id)
        Modulos->>Estagio: atualizarStatus(id, "Aprovada")
        Estagio->>DB: UPDATE solicitacoes SET status = 'Aprovada' WHERE id = ?
        DB-->>Estagio: OK
        Estagio-->>Modulos: confirmação
        Modulos-->>ProfessorService: status atualizado
        ProfessorService-->>WebApp: mostrarConfirmacao("Solicitação aprovada")
        WebApp-->>Professor: feedback de aprovação
    else Rejeitar
        Professor->>WebApp: rejeitarSolicitacao(id)
        WebApp->>ProfessorService: POST /solicitacoes/{id}/rejeitar
        ProfessorService->>Modulos: rejeitarSolicitacao(id)
        Modulos->>Estagio: atualizarStatus(id, "Rejeitada")
        Estagio->>DB: UPDATE solicitacoes SET status = 'Rejeitada' WHERE id = ?
        DB-->>Estagio: OK
        Estagio-->>Modulos: confirmação
        Modulos-->>ProfessorService: status atualizado
        ProfessorService-->>WebApp: mostrarConfirmacao("Solicitação rejeitada")
        WebApp-->>Professor: feedback de rejeição
    end
