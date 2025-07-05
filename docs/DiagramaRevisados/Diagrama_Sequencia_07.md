### UC.07 - Solicitar Professor (Aluno) com Arquitetura

```mermaid
sequenceDiagram
    actor Aluno

    participant WebApp as WebApp (Apresentação)
    participant AlunoService as AlunoService (Controller)
    participant Modulos as Gerenciador de Módulos
    participant TCCModulo as Módulo TCC
    participant DB as DatabaseService
    participant DocDB as Doc DB

    %% 1. Envio da solicitação via WebApp
    Aluno->>WebApp: selecionarProfessor(professorId)
    WebApp->>AlunoService: POST /tcc/vincularProfessor(idAluno, professorId)

    %% 2. Encaminhamento ao módulo TCC
    AlunoService->>Modulos: requisitarVinculoProfessor(idAluno, professorId)
    Modulos->>TCCModulo: vincularProfessorTCC(idAluno, professorId)

    %% 3. Verificação da disponibilidade do professor
    TCCModulo->>DB: contarOrientandos(professorId)
    DB->>DocDB: SELECT COUNT(*) FROM tcc WHERE professor_id = ?
    
    alt Professor indisponível (acima do limite)
        DocDB-->>DB: limite excedido
        DB-->>TCCModulo: indisponível
        TCCModulo-->>Modulos: erro("Professor excedeu limite")
        Modulos-->>AlunoService: erro
        AlunoService-->>WebApp: exibirErro("Professor já possui número máximo de orientandos")
        WebApp-->>Aluno: erro exibido
    else Professor disponível
        DocDB-->>DB: dentro do limite
        DB-->>TCCModulo: disponível

        %% 4. Vínculo com TCC
        TCCModulo->>DB: atualizarTCC(idAluno, professorId)
        DB->>DocDB: UPDATE tcc SET professor_id = ? WHERE aluno_id = ?
        DocDB-->>DB: OK
        DB-->>TCCModulo: vínculo realizado

        %% 5. Confirmação
        TCCModulo-->>Modulos: sucesso
        Modulos-->>AlunoService: professor vinculado
        AlunoService-->>WebApp: exibirSucesso("Professor vinculado com sucesso")
        WebApp-->>Aluno: confirmação exibida
    end
