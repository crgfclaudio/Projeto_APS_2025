```mermaid
sequenceDiagram
    actor Aluno
    participant Web/MobileApp
    participant UC_GestaoEstagio as "Controle Gestão Estágio"
    participant ServicoEstagio as "Serviço Estágio"
    participant RepositorioEstagio as "Repositório Estágio"
    participant ServicoNotificacao as "Serviço Notificação"
    participant RepositorioUsuario as "Repositório Usuário"

    Aluno->>Web/MobileApp: Acessa "Cadastrar Estágio"
    Web/MobileApp->>Aluno: Exibe formulário de cadastro
    Aluno->>Web/MobileApp: Preenche dados (título, descrição, empresa, datas) e envia
    Web/MobileApp->>UC_GestaoEstagio: cadastrarEstagio(dadosEstagio, idAluno)
    UC_GestaoEstagio->>ServicoEstagio: criarEstagio(dadosEstagio, idAluno)
    ServicoEstagio->>ServicoEstagio: validarDadosEstagio(dadosEstagio)
    alt Dados Válidos
        ServicoEstagio->>RepositorioEstagio: salvarEstagio(novoEstagio)
        RepositorioEstagio-->>ServicoEstagio: idEstagio
        ServicoEstagio-->>UC_GestaoEstagio: sucessoCadastroEstagio(idEstagio)
        UC_GestaoEstagio-->>Web/MobileApp: exibeMensagemSucesso("Estágio cadastrado com sucesso!")

        Note over UC_GestaoEstagio,ServicoNotificacao: Notificação para Coordenador/Professor
        UC_GestaoEstagio->>RepositorioUsuario: buscarUsuarios(tipoPerfil=Coordenador/Professor)
        RepositorioUsuario-->>UC_GestaoEstagio: listaCoordenadoresProfessores(usuarios)
        UC_GestaoEstagio->>ServicoNotificacao: enviarNotificacao(usuarios, "Novo Estágio para Aprovação", "Um novo estágio foi cadastrado e aguarda sua aprovação.")
        ServicoNotificacao-->>UC_GestaoEstagio: notificacaoEnviada()
    else Dados Inválidos
        ServicoEstagio-->>UC_GestaoEstagio: erroValidacao(mensagensErro)
        UC_GestaoEstagio-->>Web/MobileApp: exibeMensagemErro(mensagensErro)
    end
