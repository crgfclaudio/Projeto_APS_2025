### UC.10 - Ver Lista de Monitorias Ofertadas com Arquitetura
```mermaid
sequenceDiagram
    actor Aluno

    participant WebApp as WebApp (Apresentação)
    participant AlunoService as AlunoService (Controller)
    participant Modulos as Gerenciador de Módulos
    participant Monitoria as Módulo Monitoria
    participant DB as DatabaseService
    participant DocDB as Doc DB

    %% 1. Início da requisição via frontend
    Aluno->>WebApp: acessarListaMonitorias(filtros)
    WebApp->>AlunoService: requisitarMonitorias(filtros)

    %% 2. Encaminhamento via módulo
    AlunoService->>Modulos: encaminharParaMonitoria(fi
