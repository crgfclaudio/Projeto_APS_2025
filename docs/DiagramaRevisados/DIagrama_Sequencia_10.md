### UC.10 - Ver Lista de Monitorias Ofertadas
```mermaid
sequenceDiagram
    actor Aluno
    participant MonitoriaListagem as MonitoriaListagem (Boundary)
    participant MonitoriaConsultaController as MonitoriaConsultaController (Control)
    participant MonitoriaCollection as MonitoriaCollection (EntityCollection)
    participant DB as PostgreSQL

    %% 1. Requisição da listagem
    Aluno->>MonitoriaListagem: acessarLista(filtros)
    MonitoriaListagem->>MonitoriaConsultaController: listarMonitorias(filtros)

    %% 2. Consulta no repositório
    MonitoriaConsultaController->>MonitoriaCollection: buscarDisponiveis(filtros)
    MonitoriaCollection->>DB: SELECT * FROM monitorias WHERE status = 'aberta' AND filtros...
    DB-->>MonitoriaCollection: lista de monitorias
    MonitoriaCollection-->>MonitoriaConsultaController: monitoriasEncontradas
    MonitoriaConsultaController-->>MonitoriaListagem: exibirLista(monitorias)
    MonitoriaListagem-->>Aluno: mostrarMonitoriasDisponiveis()
