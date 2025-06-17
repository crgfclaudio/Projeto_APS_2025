classDiagram
    class Usuario {
        +id: String
        +nome: String
        +email: String
        +senha: String
        +tipoPerfil: Enum
        +matricula: String [0..1]
        +departamento: String [0..1]
    }

    class Estagio {
        +id: String
        +titulo: String
        +descricao: String
        +dataInicio: Date
        +dataFim: Date
        +empresa: String
        +status: Enum
    }

    class RelatorioEstagio {
        +id: String
        +titulo: String
        +conteudo: String
        +dataSubmissao: Date
        +status: Enum
    }

    class Monitoria {
        +id: String
        +titulo: String
        +descricao: String
        +disciplina: String
        +dataAbertura: Date
        +dataFechamento: Date
        +status: Enum
    }

    class CandidaturaMonitoria {
        +id: String
        +dataCandidatura: Date
        +status: Enum
    }

    class Documento {
        +id: String
        +tipo: Enum
        +nomeArquivo: String
        +urlDownload: String
        +dataGeracao: Date
        +codigoAutenticacao: String
    }

    class Notificacao {
        +id: String
        +titulo: String
        +mensagem: String
        +dataEnvio: Date
        +lida: Boolean
        +tipo: Enum
    }

    class LogAuditoria {
        +id: String
        +dataHora: DateTime
        +usuario: String
        +acao: String
        +detalhes: String
    }

    Usuario "1" -- "0..*" Estagio : realiza
    Estagio "1" -- "0..*" RelatorioEstagio : possui
    Usuario "1" -- "0..*" RelatorioEstagio : submete / aprova
    Usuario "1" -- "0..*" Monitoria : se candidata a
    Monitoria "1" -- "0..*" CandidaturaMonitoria : possui
    Usuario "1" -- "0..*" Notificacao : recebe
    Usuario "1" -- "0..*" Documento : gera
    Usuario "1" -- "0..*" LogAuditoria : realiza

    CandidaturaMonitoria "1" -- "1" Usuario : candidata_de
    CandidaturaMonitoria "1" -- "1" Monitoria : para_monitoria
    RelatorioEstagio "1" -- "1" Usuario : submetido_por
    RelatorioEstagio "1" -- "1" Estagio : referente_a
