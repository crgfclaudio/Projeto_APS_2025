
```mermaid
graph TB
  %% Camada de Apresentação
  subgraph Apresentação
    WebUI[Web App]
    MobileUI[Mobile App]
  end

  %% Camada de Negócio (Microserviços)
  subgraph Negócio
    APIGateway[API Gateway]
    AuthService[Auth Service]
    ProductService[Product Service]
    OrderService[Order Service]
  end

  %% Camada de Persistência
  subgraph Dados
    UserDB[(User DB)]
    ProductDB[(Product DB)]
    OrderDB[(Order DB)]
    MessageBroker[(RabbitMQ)]
  end

  %% Fluxos principais
  WebUI -->|HTTPS| APIGateway
  MobileUI -->|HTTPS| APIGateway

  APIGateway --> AuthService
  APIGateway --> ProductService
  APIGateway --> OrderService

  AuthService -->|CRUD| UserDB
  ProductService -->|CRUD| ProductDB
  OrderService -->|CRUD| OrderDB

  ;; Comunicação assíncrona (e-mail, notificações, etc.)
  OrderService -->|pub/sub| MessageBroker
  MessageBroker -->|notify| EmailWorker[(Email Worker)]

  %% Componente de envio de e-mail
  subgraph “Serviços Auxiliares”
    EmailWorker
  end
