### UC.25 - Abrir Monitoria (Professor)

```mermaid
sequenceDiagram
    actor Professor
    participant PainelProfessor as UI: PainelProfessor (React)
    participant MonitoriaController as Controller: MonitoriaController (Express/Django)
    participant MonitoriaService as Service: MonitoriaService
    participant Monitoria as Entity: Monitoria
    participant MonitoriaRepository as Repository: MonitoriaRepository
    participant DB as PostgreSQL

    Professor->>PainelProfessor: acessar formulário de abertura
    PainelProfessor->>Professor: exibirFormulário()
    Professor->>PainelProfessor: preencher e submeter(dados)

    PainelProfessor->>MonitoriaController: POST /monitoria (dados)
    MonitoriaController->>MonitoriaService: validar e processar(dados)
    
    alt Dados inválidos
        MonitoriaService-->>MonitoriaController: erro de validação
        MonitoriaController-->>PainelProfessor: exibirErro("Dados inválidos")
    else Dados válidos
        MonitoriaService->>Monitoria: criar instância(disciplina, vagas, periodo, critérios)
        MonitoriaService->>MonitoriaRepository: verificarDuplicidade(monitoria)
        MonitoriaRepository->>DB: SELECT * FROM monitorias WHERE ...
        alt Monitoria duplicada
            DB-->>MonitoriaRepository: já existe
            MonitoriaRepository-->>MonitoriaService: duplicado
            MonitoriaService-->>MonitoriaController: erro de duplicidade
            MonitoriaController-->>PainelProfessor: exibirErro("Monitoria já cadastrada")
        else Monitoria nova
            DB-->>MonitoriaRepository: nenhum resultado
            MonitoriaRepository->>DB: INSERT INTO monitorias VALUES (...)
            DB-->>MonitoriaRepository: confirmação
            MonitoriaRepository-->>MonitoriaService: sucesso
            MonitoriaService-->>MonitoriaController: sucesso
            MonitoriaController-->>PainelProfessor: exibirSucesso("Monitoria aberta com sucesso")
        end
    end
