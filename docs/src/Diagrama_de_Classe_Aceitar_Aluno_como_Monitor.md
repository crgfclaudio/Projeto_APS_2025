### UC.26 - Aceitar Aluno como Monitor (Professor)

```mermaid
%% Diagrama de Classe para UC.26 - Aceitar Aluno como Monitor
classDiagram
    class PainelProfessor {
        <<boundary>>
        +visualizarCandidatos()
        +aceitarAluno()
    }

    class CandidaturaMonitoriaController {
        <<control>>
        +validarCandidato()
        +aceitarCandidatura()
    }

    class Candidatura {
        <<entity>>
        +idAluno: String
        +idMonitoria: String
        +status: String
    }

    class CandidaturaCollection {
        <<entity collection>>
        +buscarPendentes()
        +atualizarStatus()
    }

    PainelProfessor --> CandidaturaMonitoriaController : seleciona aluno
    CandidaturaMonitoriaController --> CandidaturaCollection : aprova
