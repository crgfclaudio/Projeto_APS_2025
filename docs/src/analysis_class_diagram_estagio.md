```mermaid
classDiagram
    direction LR

    class Aluno {
        + id: String
        + nome: String
        + matricula: String
    }

    class Estagio {
        + id: String
        + titulo: String
        + descricao: String
        + empresa: String
        + dataInicio: Date
        + dataFim: Date
        + status: String (Aguardando Aprovação, Ativo, Concluído, Reprovado)
        + cargaHoraria: Int
        + area: String
    }

    class RelatorioEstagio {
        + id: String
        + titulo: String
        + dataSubmissao: Date
        + anexoURL: String
        + observacoes: String
        + status: String (Pendente, Aprovado, Reprovado)
    }

    class Professor {
        + id: String
        + nome: String
        + departamento: String
    }

    class Coordenador {
        + id: String
        + nome: String
        + curso: String
    }

    Aluno "1" -- "0..*" Estagio : realiza
    Estagio "1" -- "0..*" RelatorioEstagio : possui
    Professor "1" -- "0..*" Estagio : orienta
    Coordenador "1" -- "0..*" Estagio : gerencia
    Coordenador "1" -- "0..*" RelatorioEstagio : avalia

    Aluno "1" -- "0..*" Estagio : realiza
    Estagio "1" -- "0..*" RelatorioEstagio : possui
    Professor "1" -- "0..*" Estagio : orienta
    Coordenador "1" -- "0..*" Estagio : gerencia
    Coordenador "1" -- "0..*" RelatorioEstagio : avalia
