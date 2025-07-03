# Diagrama de Casos de Uso - Sistema ConectaAE (Orientação Esquerda-Direita)

Este diagrama representa os principais atores e casos de uso do sistema Plataforma Acadêmica Integrada ConectaAE.

```mermaid
graph LR
%%Atores
    actor A[Aluno]
    actor P[Professor]
    actor ADM[Administrador]
    actor C[Coordenador]
    actor UNL[Usuário Não Logado]

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
    UNL -- realiza --> UC1
    UNL -- realiza --> UC3

    A -- realiza --> UC1
    A -- realiza --> UC_TCC_1
    A -- realiza --> UC_TCC_2
    A -- realiza --> UC_TCC_5
    A -- realiza --> UC_Estagio_1
    A -- realiza --> UC_Estagio_2
    A -- realiza --> UC_Monitoria_2
    A -- realiza --> UC_Monitoria_4

    P -- realiza --> UC1
    P -- realiza --> UC_TCC_3
    P -- realiza --> UC_Estagio_3
    P -- realiza --> UC_Monitoria_1
    P -- realiza --> UC_FG_2

    ADM -- realiza --> UC1
    ADM -- realiza --> UC2
    ADM -- realiza --> UC_TCC_3
    ADM -- realiza --> UC_TCC_4
    ADM -- realiza --> UC_Estagio_3
    ADM -- realiza --> UC_Estagio_4
    ADM -- realiza --> UC_Monitoria_1
    ADM -- realiza --> UC_Monitoria_3
    ADM -- realiza --> UC_Monitoria_5
    ADM -- realiza --> UC_FG_1
    ADM -- realiza --> UC_FG_2
    ADM -- realiza --> UC_FG_3
    ADM -- realiza --> UC_FG_4

    C -- realiza --> UC1
    C -- realiza --> UC_TCC_3
    C -- realiza --> UC_TCC_4
    C -- realiza --> UC_Estagio_3
    C -- realiza --> UC_Estagio_4
    C -- realiza --> UC_Monitoria_1
    C -- realiza --> UC_Monitoria_3
    C -- realiza --> UC_Monitoria_5
    C -- realiza --> UC_FG_1
    C -- realiza --> UC_FG_2
    C -- realiza --> UC_FG_3
    C -- realiza --> UC_FG_4
