### UC.11 - Registrar Estágio com Arquitetura
```mermaid
sequenceDiagram
    actor Aluno

    participant WebApp as WebApp (Apresentação)
    participant AlunoService as AlunoService (Controller)
    participant Modulos as Gerenciador de Módulos
    participant Estagio as Módulo Estágio
    participant DB as DatabaseService
    participant DocDB as Doc DB

    %% 1. Entrada de dados via frontend
    Aluno->>WebApp: preencherFormularioEstagio(dados, arquivo)
    WebApp->>AlunoService: POST /estagio/registrar (dados, file)

    %% 2. Encaminhamento ao módulo de estágio
    AlunoService->>Modulos: registrarEstagio(idAluno, dados, file)
    Modulos->>Estagio: processarRegistroEstagio(idAluno, dados, file)

    %% 3. Validação de dados de entrada
    Estagio->>Estagio: validarDados(dados, file)
    alt Dados inválidos
        Estagio-->>Modulos: erro("Dados inválidos")
        Modulos-->>AlunoService: erro de validação
        AlunoService-->>WebApp: exibirErro("Dados inválidos")
        WebApp-->>Aluno: mostrarErro
    else Dados válidos

        %% 4. Regras de negócio (período, carga horária, etc.)
        Estagio->>Estagio: validarRegrasNegocio()
        alt Regras não atendidas
            Estagio-->>Modulos: erro("Regras não atendidas")
            Modulos-->>AlunoService: erro de regras
            AlunoService-->>WebApp: exibirErro("Regras não atendidas")
            WebApp-->>Aluno: mostrarErro
        else Regras válidas

            %% 5. Persistência no banco
            Estagio->>DB: salvarEstagio(dados, arquivo)
            DB->>DocDB: INSERT INTO estagios (...)
            DocDB-->>DB: OK
            DB-->>Estagio: confirmação

            %% 6. Resposta de sucesso
            Estagio-->>Modulos: sucesso
            Modulos-->>AlunoService: estágio salvo
            AlunoService-->>WebApp: exibirSucesso("Estágio registrado com sucesso")
            WebApp-->>Aluno: mostrarConfirmação
        end
    end

