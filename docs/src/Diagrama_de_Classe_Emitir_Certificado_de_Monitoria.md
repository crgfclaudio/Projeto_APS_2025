### UC.27 - Emitir Certificado de Monitoria (Professor)

```mermaid
%% Diagrama de Classe para UC.27 - Emitir Certificado de Monitoria
classDiagram
    class PainelProfessor {
        <<boundary>>
        +selecionarMonitor()
        +emitirCertificado()
    }

    class CertificadoController {
        <<control>>
        +gerarCertificado()
        +validarConclusao()
    }

    class Monitor {
        <<entity>>
        +nome: String
        +disciplina: String
        +periodo: String
    }

    class Certificado {
        <<entity>>
        +monitor: String
        +dataEmissao: Date
        +arquivo: PDF
    }

    class CertificadoCollection {
        <<entity collection>>
        +registrar()
        +emitir()
    }

    PainelProfessor --> CertificadoController : solicita emissão
    CertificadoController --> Certificado : gera
    CertificadoController --> CertificadoCollection : armazena
