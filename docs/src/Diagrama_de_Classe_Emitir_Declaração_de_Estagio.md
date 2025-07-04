### UC.28 - Emitir Declaração de Estágio (Professor)

```mermaid
%% Diagrama de Classe para UC.28 - Emitir Declaração de Estágio
classDiagram
    class PainelProfessor {
        <<boundary>>
        +selecionarEstagio()
        +emitirDeclaracao()
    }

    class EstagioDeclaracaoController {
        <<control>>
        +validarDados()
        +gerarDeclaracao()
    }

    class Estagio {
        <<entity>>
        +aluno: String
        +empresa: String
        +periodo: String
    }

    class DeclaracaoEstagio {
        <<entity>>
        +texto: String
        +data: Date
        +arquivo: PDF
    }

    class DeclaracaoEstagioCollection {
        <<entity collection>>
        +registrar()
        +emitir()
    }

    PainelProfessor --> EstagioDeclaracaoController : solicita emissão
    EstagioDeclaracaoController --> DeclaracaoEstagio : instancia
    EstagioDeclaracaoController --> DeclaracaoEstagioCollection : armazena
