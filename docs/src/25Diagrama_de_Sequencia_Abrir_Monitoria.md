### UC.25 - Abrir Monitoria (Professor)

```mermaid
sequenceDiagram
    actor Professor
    participant Sistema
    participant BancoDeDados

    Professor->>Sistema: Acessa menu "Monitoria"
    Professor->>Sistema: Seleciona "Abrir Nova Monitoria"
    Sistema-->>Professor: Exibe formulário de abertura

    Professor->>Sistema: Preenche e envia formulário (Disciplina, Vagas, Período, Critérios)
    Sistema->>Sistema: Valida dados do formulário

    alt Dados válidos e disciplina permitida
        Sistema->>BancoDeDados: Registra nova chamada de monitoria
        BancoDeDados-->>Sistema: Confirmação de registro
        Sistema-->>Professor: Exibe mensagem de sucesso
    else Dados inválidos
        Sistema-->>Professor: Exibe mensagem de erro (dados incompletos ou inválidos)
    else Disciplina não vinculada
        Sistema-->>Professor: Bloqueia ação e exibe aviso
    else Falha técnica
        Sistema-->>Professor: Exibe mensagem de erro e orienta nova tentativa
    end
```
