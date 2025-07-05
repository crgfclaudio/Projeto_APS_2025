### UC.07 - Solicitar Professor (Aluno)

```mermaid
sequenceDiagram
    actor Aluno
    participant IndicacaoProfessorForm as IndicacaoProfessorForm (Boundary)
    participant TCCController as TCCController (Control)
    participant ProfessorCollection as ProfessorCollection (EntityCollection)
    participant DB as PostgreSQL
    participant TCC as TCC (Entity)

    %% 1. Seleção de professor
    Aluno->>IndicacaoProfessorForm: selecionarProfessor(professorId)
    IndicacaoProfessorForm->>TCCController: POST /tcc/vincularProfessor(idAluno, professorId)

    %% 2. Validação da disponibilidade do professor
    TCCController->>ProfessorCollection: validarDisponibilidade(professorId)
    ProfessorCollection->>DB: SELECT COUNT(*) FROM tcc WHERE professor_id = ?
    alt Professor indisponível (acima do limite)
        DB-->>ProfessorCollection: limite excedido
        ProfessorCollection-->>TCCController: indisponível
        TCCController-->>IndicacaoProfessorForm: exibirErro("Professor já possui número máximo de orientandos")
        IndicacaoProfessorForm-->>Aluno: erro exibido
    else Professor disponível
        DB-->>ProfessorCollection: dentro do limite
        ProfessorCollection-->>TCCController: disponível

        %% 3. Vínculo com o TCC
        TCCController->>TCC: vincularProfessor(idAluno, professorId)
        TCC->>DB: UPDATE tcc SET professor_id = ? WHERE aluno_id = ?
        DB-->>TCC: OK
        TCC-->>TCCController: vínculo realizado
        TCCController-->>IndicacaoProfessorForm: notificarSucesso("Professor vinculado com sucesso")
        IndicacaoProfessorForm-->>Aluno: confirmação exibida
    end
