### UC.30 - Atribuir Nota

```mermaid
%% Diagrama de Classe para UC.30 – Atribuir Nota
classDiagram
    class PainelProfessor {
        <<boundary>>
        + selecionarAcompanhamento(idAcompanhamento)
        + visualizarRelatorios()
        + selecionarRelatorio(idRelatorio)
        + enviarAvaliacao(nota, comentario)
    }

    class AvaliacaoController {
        <<control>>
        + fetchRelatorios(idAcompanhamento)
        + fetchRelatorio(idRelatorio)
        + registrarAvaliacao(idRelatorio, nota, comentario)
        + notificarAluno(idRelatorio)
    }

    class Relatorio {
        <<entity>>
        + id: String
        + tipo: String
        + dataEnvio: Date
        + arquivo: File
        + status: String
    }

    class RelatorioCollection {
        <<entity collection>>
        + listarPorAcompanhamento(idAcompanhamento)
        + buscarPorId(idRelatorio)
    }

    class Avaliacao {
        <<entity>>
        + idRelatorio: String
        + nota: Float
        + comentarios: String
        + data: Date
    }

    class AvaliacaoCollection {
        <<entity collection>>
        + adicionar(avaliacao)
    }

    PainelProfessor --> AvaliacaoController : selecionarRelatorio(idRelatorio)
    PainelProfessor --> AvaliacaoController : enviarAvaliacao(nota, comentario)
    AvaliacaoController --> RelatorioCollection : listarPorAcompanhamento(idAcompanhamento)
    RelatorioCollection --> Relatorio : retornaLista()
    AvaliacaoController --> Relatorio : fetchRelatorio(idRelatorio)
    AvaliacaoController --> AvaliacaoCollection : adicionar(avaliacao)
    AvaliacaoController --> PainelProfessor : mostrarConfirmacao()

        +atualizarStatus()
    }

    PainelProfessor --> LiberacaoTCCController : solicita liberação
    LiberacaoTCCController --> TCCCollection : atualiza status
