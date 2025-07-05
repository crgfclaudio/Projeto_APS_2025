### UC.9 - Se Candidatar à Monitoria com arquitetura
```mermaid
sequenceDiagram
    actor Aluno

    participant WebApp as WebApp (Apresentação)
    participant Auth as AuthService
    participant AlunoService as AlunoService
    participant Modulos as Gerenciador de Módulos
    participant Monitoria as Módulo Monitoria
    participant DB as DatabaseService
    participant MQ as RabbitMQ
    participant Email as Sistema de e-mail

    %% 1. Autenticação
    Aluno->>WebApp: acessar candidatura
    WebApp->>Auth: autenticar(login/sessão)
    Auth-->>WebApp: token válido

    %% 2. Preenchimento do formulário
    Aluno->>WebApp: submeterFormularioCandidatura(dados)
    WebApp->>AlunoService: POST /monitoria/candidatar (dados,



