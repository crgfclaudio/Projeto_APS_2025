# Projeto_APS_2025
Projeto  a ser feito para a aula de Analise e Projeto de Software
Arquitetura do Sistema
# Pastas
- UC->todos os casos de uso do sistema
- docs/src->todos os diagramas de classe e sequencia feitos antes da arquitetura do projeto
- docs/DiagramaRevisados->diagramas feitos pelo grupo revisados após a arquitetura 
# 📐 Projeto Arquitetural — Sistema ConectaAE

## 1. Visão Geral
O **ConectaAE** é uma plataforma web voltada à gestão de atividades extracurriculares da Universidade de Pernambuco (UPE), integrando módulos como TCC, Estágio, Monitoria e Relatórios, com suporte a diferentes perfis de usuários: **Aluno**, **Professor**, **Monitor** e **Administrador**. Cada perfil possui permissões específicas dentro do sistema.



---

## 2. Padrão Arquitetural Adotado
- **Arquitetura baseada em Microserviços**

Dividida em 3 camadas principais:

Camada de Apresentação

Camada de Negócio

Camada de Dados

Comunicação via **API Gateway** e **RabbitMQ** (mensageria)

Organização interna dos serviços com o padrão **MVC**

---

## 3. Camadas da Arquitetura

### 🎨 A. Camada de Apresentação (Frontend)
- **Tecnologia:** React.js + TailwindCSS
- Acesso via navegador (WebApp)
- Interfaces distintas para usuários **logados** e **não logados**
- Comunicação com backend via **HTTPS**
### 🧠 B. Camada de Lógica de Negócio (Backend)
Serviços desacoplados sob um **API Gateway**, cada um responsável por um domínio funcional:
| Serviço	                      | Responsável por                                             |
| ------------------------------|-------------------------------------------------------------|
| Auth Service	                | Login, logout, controle de sessão                           |
| User Service	                | CRUD de perfis: aluno, professor, monitor, administrador    | 
| Aluno Service	                | Submissões, matrículas, certificados, interações nos módulos|
| Professor Service	            | Avaliação de relatórios, emissão de notas e comentários     |
| ADM Service	                  | Administração geral do sistema                              |
| Gerenciador de Módulos	      | Coordenação de módulos TCC, Estágio, Monitoria e Relatório  |

### 💾 C. Camada de Dados (Persistência)
Gerenciado por um **Database Service**, que interage com os repositórios:
| Recurso	                      | Função                                                      |
|-------------------------------|-------------------------------------------------------------|
| User DB	                      | Armazenamento de dados de usuários                          |
| Doc DB	                      | Armazenamento de documentos, relatórios e avaliações        |
| RabbitMQ	                    | Canal assíncrono para notificações e integração com e-mails |

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

