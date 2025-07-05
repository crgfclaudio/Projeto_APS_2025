### UC.10 - Ver Lista de Monitorias Ofertadas
```mermaid
classDiagram
    class MonitoriaListagem {
        <<boundary>>
        +listarVagas()
    }

    class MonitoriaConsultaController {
        <<control>>
        +buscarMonitoriasAbertas()
    }

    class Monitoria {
        <<entity>>
        +disciplina: String
        +professor: String
        +vagasDisponiveis: int
        +periodo: String
    }

    class MonitoriaCollection {
        <<entity collection>>
        +listarDisponiveis()
        +filtrar(filtros)
    }

    MonitoriaListagem --> MonitoriaConsultaController : requisita dados
    MonitoriaConsultaController --> MonitoriaCollection : consulta
    MonitoriaCollection --> Monitoria : retorna entidades
    MonitoriaConsultaController --> MonitoriaListagem : exibe lista
