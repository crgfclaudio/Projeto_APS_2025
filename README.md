# Projeto_APS_2025
Projeto  a ser feito para a aula de Analise e Projeto de Software
Arquitetura do Sistema
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
graph TD
  subgraph Frontend
    A1[Interface Web (React)]
  end

  subgraph Backend
    B1[API REST (Express.js)]
    B2[Controladores MVC]
    B3[Serviços (Negócio)]
    B4[Gerenciador de Sessões]
  end

  subgraph Database
    C1[(PostgreSQL)]
  end

  A1 --> B1
  B1 --> B2
  B2 --> B3
  B3 --> C1
  B1 --> B4
