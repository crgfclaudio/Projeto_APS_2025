sequenceDiagram
    actor Aluno
    participant Web/MobileApp
    participant UC_GestaoEstagio as "Controle Gestão Estágio"
    participant ServicoRelatorio as "Serviço Relatório"
    participant RepositorioRelatorio as "Repositório Relatório"
    participant ServicoNotificacao as "Serviço Notificação"
    participant RepositorioUsuario as "Repositório Usuário"

    Aluno->>Web/MobileApp: Acessa "Meus Estágios"
    Web/MobileApp->>UC_GestaoEstagio: buscarEstagiosAluno(idAluno)
    UC_GestaoEstagio-->>Web/MobileApp: listaEstagios(estagios)
    Aluno->>Web/MobileApp: Seleciona Estágio e "Submeter Relatório"
    Web/MobileApp->>Web/MobileApp: Exibe formulário de submissão
    Aluno->>Web/MobileApp: Preenche dados (título, anexo) e envia
    Web/MobileApp->>UC_GestaoEstagio: submeterRelatorio(idEstagio, dadosRelatorio, idAluno)
    UC_GestaoEstagio->>ServicoRelatorio: criarRelatorio(idEstagio, dadosRelatorio, idAluno)
    ServicoRelatorio->>RepositorioRelatorio: salvarRelatorio(novoRelatorio)
    RepositorioRelatorio-->>ServicoRelatorio: relatorioSalvo(idRelatorio)
    ServicoRelatorio-->>UC_GestaoEstagio: relatorioCriado(idRelatorio)
    UC_GestaoEstagio-->>Web/MobileApp: sucessoSubmissao()
    Web/MobileApp-->>Aluno: Exibe mensagem "Relatório submetido com sucesso!"

    Note over UC_GestaoEstagio,ServicoNotificacao: Notificação para Coordenador/Professor
    UC_GestaoEstagio->>RepositorioUsuario: buscarUsuarios(tipoPerfil=Coordenador/Professor)
    RepositorioUsuario-->>UC_GestaoEstagio: listaCoordenadoresProfessores(usuarios)
    UC_GestaoEstagio->>ServicoNotificacao: enviarNotificacao(usuarios, "Novo Relatório para Aprovação", "Um novo relatório de estágio foi submetido.")
    ServicoNotificacao-->>UC_GestaoEstagio: notificacaoEnviada()
