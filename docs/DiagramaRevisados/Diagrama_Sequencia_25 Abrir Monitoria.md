### UC.25 - Abrir Monitoria (Professor)

```mermaid
sequenceDiagram
    actor Professor

    participant WebApp as WebApp (Apresentação)
    participant ProfessorService as ProfessorService (Controller)
    participant Modulos as Gerenciador de Módulos
    participant Monitoria as Módulo Monitoria
    participant DB as DatabaseService
    participant DocDB as Doc DB

    %% 1. Acesso ao formulário
    Professor->>WebApp: acessarFormularioAbertura()
    WebApp->>Professor: exibirFormulario()

    %% 2. Submissão da solicitação
    Professor->>WebApp: preencherEsubmeter(dados)
    WebApp->>ProfessorService: POST /monitoria (dados)
    ProfessorService->>Modulos: abrirMonitoria(dados)
    Modulos->>Monitoria: validarERegistrar(dados)

    %% 3. Validação de dados
    Monitoria->>Monitoria: validarCampos(dados)
    alt Dados inválidos
        Monitoria-->>Modulos: erro("Dados inválidos")
        Modulos-->>ProfessorService: erro validação
        ProfessorService-->>WebApp: exibirErro("Dados inválidos")
        WebApp-->>Professor: feedback erro
    else Dados válidos
        %% 4. Verificação de duplicidade
        Monitoria->>DB: verificarDuplicidade(dados)
        DB->>DocDB: SELECT * FROM monitorias WHERE disciplina = ... AND periodo = ...
        alt Monitoria duplicada
            DocDB-->>DB: já existe
            DB-->>Monitoria: duplicado
            Monitoria-->>Modulos: erro("Monitoria já cadastrada")
            Modulos-->>ProfessorService: erro duplicidade
            ProfessorService-->>WebApp: exibirErro("Monitoria já cadastrada")
            WebApp-->>Professor: feedback erro
        else Monitoria nova
            DocDB-->>DB: nenhum resultado
            DB-->>Monitoria: ok

            %% 5. Registro no banco
            Monitoria->>DB: salvarMonitoria(dados)
            DB->>DocDB: INSERT INTO monitorias VALUES (...)
            DocDB-->>DB: OK
            DB-->>Monitoria: sucesso

            Monitoria-->>Modulos: sucesso
            Modulos-->>ProfessorService: monitoria aberta
            ProfessorService-->>WebApp: exibirSucesso("Monitoria aberta com sucesso")
            WebApp-->>Professor: feedback sucesso
        end
    end

            MonitoriaRepository-->>MonitoriaService: sucesso
            MonitoriaService-->>MonitoriaController: sucesso
            MonitoriaController-->>PainelProfessor: exibirSucesso("Monitoria aberta com sucesso")
        end
    end
