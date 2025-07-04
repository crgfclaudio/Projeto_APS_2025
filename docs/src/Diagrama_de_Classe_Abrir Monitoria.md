### UC.25 - Abrir Monitoria (Professor)

```mermaid
%% Diagrama de Classe para UC.25 - Abrir Monitoria
classDiagram
    class PainelProfessor {
        <<boundary>>
        +abrirChamada()
        +preencherFormulario()
    }

    class MonitoriaController {
        <<control>>
        +validarDados()
        +criarMonitoria()
    }

    class Monitoria {
        <<entity>>
        +disciplina: String
        +vagas: int
        +periodo: String
        +criterios: String
    }

    class MonitoriaCollection {
        <<entity collection>>
        +registrarMonitoria()
        +verificarDuplicidade()
    }

    PainelProfessor --> MonitoriaController : envia dados
    MonitoriaController --> Monitoria : instancia
    MonitoriaController --> MonitoriaCollection : armazena
