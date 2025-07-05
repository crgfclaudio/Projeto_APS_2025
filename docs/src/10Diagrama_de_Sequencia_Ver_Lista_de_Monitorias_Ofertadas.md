### UC.10 - Ver Lista de Monitorias Ofertadas
```mermaid
sequenceDiagram
    participant Listagem as MonitoriaListagem
    participant Controller as MonitoriaConsultaController
    participant Collection as MonitoriaCollection
    participant Entidade as Monitoria

    Listagem->>Controller: requisita dados
    Controller->>Collection: listarDisponiveis() / filtrar(filtros)
    Collection->>Entidade: busca monitorias abertas
    Collection-->>Controller: retorna lista de Monitoria
    Controller-->>Listagem: exibe lista
