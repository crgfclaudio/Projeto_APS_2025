```mermaid
graph TB

%% Camada de Apresentação
subgraph Apresentação
  WebApp[WebApp]
  WebApp --> NaoLogado[Não Logado]
  WebApp --> Logado[Logado]
end

WebApp -->|HTTPS| APIGateway

%% Camada de Negócio
subgraph "Negócio (Backend)"
  APIGateway

  AuthService[Auth Service]
  UserService[User Service]
  AlunoService[Aluno Service]
  ProfessorService[Professor Service]
  ADMService[ADM Service]

  ModuloManager[Gerenciador de Módulos]
  ModuloManager --> ModTCC[Módulo TCC]
  ModuloManager --> ModEstagio[Módulo Estágio]
  ModuloManager --> ModMonitoria[Módulo Monitoria]
  ModuloManager --> ModRelatorio[Módulo Relatório]
end

APIGateway --> AuthService
APIGateway --> UserService
APIGateway --> AlunoService
APIGateway --> ProfessorService
APIGateway --> ADMService

AlunoService --> ModuloManager
ProfessorService --> ModuloManager
ADMService --> ModuloManager

AuthService --> DatabaseService
UserService --> DatabaseService
ModuloManager --> DatabaseService

%% Camada de Dados
subgraph "Dados"
  DatabaseService

  DBUser[(User DB)]
  DBDoc[(Doc DB)]
  MQ[(RabbitMQ)]
end

DatabaseService -->|CRUD| DBUser
DatabaseService -->|CRUD| DBDoc
DatabaseService -->|pub/sub| MQ

%% Serviços Externos
MQ --> EmailSystem[Sistema de e-mail]
