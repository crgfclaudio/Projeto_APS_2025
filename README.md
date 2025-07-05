# Projeto_APS_2025
Projeto  a ser feito para a aula de Analise e Projeto de Software
Arquitetura do Sistema
# Pasta
- UC->todos os casos de uso do sistema
- docs/src->todos os diagramas de classe e sequencia feitos antes da arquitetura do projeto
- docs/DiagramaRevisados->diagramas feitos pelo grupo revisados após a arquitetura 
# 📐 Projeto Arquitetural — Sistema ConectaAE

## 1. Visão Geral
O **ConectaAE** é uma plataforma web voltada para a gestão de atividades acadêmicas extracurriculares da UPE, integrando funcionalidades relacionadas a **TCC**, **Estágio** e **Monitoria**. O sistema possui diferentes perfis de usuários (Aluno, Professor, Monitor, Administrador), cada um com permissões específicas.

---

## 2. Padrão Arquitetural Adotado
- **Arquitetura em Camadas (Three-Tier Architecture)** com **Padrão MVC (Model-View-Controller)**.
- Organização em:
  - Interface (Frontend)
  - Lógica de Negócio (Backend)
  - Persistência de Dados (Banco de Dados)

---

## 3. Camadas da Arquitetura

### 🎨 A. Camada de Apresentação (Frontend)
- **Tecnologia:** React.js com TailwindCSS
- **Responsável por:**
  - Renderizar a interface do usuário
  - Capturar entradas (ex: formulários de relatório)
  - Exibir respostas do backend
  - Responsividade para desktop e mobile

### 🧠 B. Camada de Lógica de Negócio (Backend)
- **Tecnologia:** Node.js com Express (ou Django)
- **Responsável por:**
  - Autenticação/autorização
  - Regras de negócio (atribuir nota, aprovar estágio, etc.)
  - Validação de dados
  - Emissão de notificações
  - Comunicação com o banco de dados

### 💾 C. Camada de Dados (Persistência)
- **Tecnologia:** PostgreSQL
- **Responsável por:**
  - Armazenar dados dos usuários, relatórios, documentos, avaliações
  - Gerenciar transações e integridade

---

## 4. Componentes do Sistema

| Componente                | Responsável por                                                  |
|---------------------------|------------------------------------------------------------------|
| Autenticação              | Login, logout, controle de sessão                               |
| Gestão de Perfis          | CRUD de aluno, professor, monitor, administrador                |
| Módulo de TCC             | Envio de arquivos, avaliações, publicação                       |
| Módulo de Estágio         | Relatórios, declarações, aprovações                             |
| Módulo de Monitoria       | Matrícula, aceite, emissão de certificados                      |
| Avaliação de Relatórios   | Notas e comentários feitos por professores                      |
| Notificações              | Envio de alertas e mensagens ao usuário                         |
| Painel Administrativo     | Administração de usuários e conteúdos                           |

---

## 5. Diagrama de Componentes

```mermaid
graph TB
  %% Camada de Apresentação
  subgraph Apresentação
    WebUI[Web App]
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

  APIGateway --> AuthService
  APIGateway --> ProductService
  APIGateway --> OrderService

  AuthService -->|CRUD| UserDB
  ProductService -->|CRUD| ProductDB
  OrderService -->|CRUD| OrderDB

  %% Comunicação assíncrona (e-mail, notificações, etc.)
  OrderService -->|pub/sub| MessageBroker
  MessageBroker -->|notify| EmailWorker[Email Worker]

  %% Serviços Auxiliares
  subgraph "Serviços Auxiliares"
    EmailWorker
  end
