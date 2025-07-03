```mermaid
graph LR
    %% Atores (agora usando a palavra-chave 'actor' para exibir o boneco)
    actor Aluno
    actor Professor
    actor Administrador
    actor Coordenador
    actor UsuarioNaoLogado

    %% Casos de Uso (baseados nas funcionalidades e requisitos do SRS)
    subgraph Sistema ConectaAE
        UC1(Logar)
        UC2(Registrar Usuário)
        UC3(Recuperar Senha)

        subgraph Gestão de TCC
            UC_TCC_1(Submeter Relatório Parcial TCC)
            UC_TCC_2(Submeter Relatório Final TCC)
            UC_TCC_3(Acompanhar e Avaliar TCC)
            UC_TCC_4(Publicar TCC)
            UC_TCC_5(Consultar Histórico de Versões TCC)
        end

        subgraph Gestão de Estágio
            UC_Estagio_1(Cadastrar Estágio)
            UC_Estagio_2(Submeter Relatório de Estágio)
            UC_Estagio_3(Aprovar Relatório de Estágio)
            UC_Estagio_4(Emitir Declaração de Estágio)
        end

        subgraph Gestão de Monitoria
            UC_Monitoria_1(Gerenciar Chamadas de Monitoria)
            UC_Monitoria_2(Candidatar-se à Monitoria)
            UC_Monitoria_3(Matricular Aluno em Monitoria)
            UC_Monitoria_4(Consultar FAQ da Monitoria)
            UC_Monitoria_5(Emitir Certificado de Monitoria)
        end

        subgraph Funções Gerais
            UC_FG_1(Gerar Certificados e Declarações)
            UC_FG_2(Enviar Notificações por Email)
            UC_FG_3(Visualizar Relatórios Gerenciais)
            UC_FG_4(Visualizar Logs de Auditoria)
        end
    end

    %% Relacionamentos
    UsuarioNaoLogado -- realiza --> UC1
    UsuarioNaoLogado -- realiza --> UC3

    Aluno -- realiza --> UC1
    Aluno -- realiza --> UC_TCC_1
    Aluno -- realiza --> UC_TCC_2
    Aluno -- realiza --> UC_TCC_5
    Aluno -- realiza --> UC_Estagio_1
    Aluno -- realiza --> UC_Estagio_2
    Aluno -- realiza --> UC_Monitoria_2
    Aluno -- realiza --> UC_Monitoria_4

    Professor -- realiza --> UC1
    Professor -- realiza --> UC_TCC_3
    Professor -- realiza --> UC_Estagio_3
    Professor -- realiza --> UC_Monitoria_1
    Professor -- realiza --> UC_FG_2

    Administrador -- realiza --> UC1
    Administrador -- realiza --> UC2
    Administrador -- realiza --> UC_TCC_3
    Administrador -- realiza --> UC_TCC_4
    Administrador -- realiza --> UC_Estagio_3
    Administrador -- realiza --> UC_Estagio_4
    Administrador -- realiza --> UC_Monitoria_1
    Administrador -- realiza --> UC_Monitoria_3
    Administrador -- realiza --> UC_Monitoria_5
    Administrador -- realiza --> UC_FG_1
    Administrador -- realiza --> UC_FG_2
    Administrador -- realiza --> UC_FG_3
    Administrador -- realiza --> UC_FG_4

    Coordenador -- realiza --> UC1
    Coordenador -- realiza --> UC_TCC_3
    Coordenador -- realiza --> UC_TCC_4
    Coordenador -- realiza --> UC_Estagio_3
    Coordenador -- realiza --> UC_Estagio_4
    Coordenador -- realiza --> UC_Monitoria_1
    Coordenador -- realiza --> UC_Monitoria_3
    Coordenador -- realiza --> UC_Monitoria_5
    Coordenador -- realiza --> UC_FG_1
    Coordenador -- realiza --> UC_FG_2
    Coordenador -- realiza --> UC_FG_3
    Coordenador -- realiza --> UC_FG_4
    Coordenador -- realiza --> UC_Monitoria_5
    Coordenador -- realiza --> UC_FG_1
    Coordenador -- realiza --> UC_FG_2
    Coordenador -- realiza --> UC_FG_3
    Coordenador -- realiza --> UC_FG_4
