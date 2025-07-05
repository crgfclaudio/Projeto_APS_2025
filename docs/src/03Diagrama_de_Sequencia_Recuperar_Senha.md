### UC.3 - Recuperar Senha
```mermaid
sequenceDiagram

    participant Form as RecuperacaoForm
    participant Controller as RecuperacaoController
    participant Collection as UsuarioCollection
    participant User as Usuario

    Form->>Controller: submete(email)
    Controller->>Collection: buscarPorEmail(email)
    Collection->>User: verifica existência
    Collection-->>Controller: retorna usuário (ou null)
    Controller->>Form: envia feedback (sucesso ou erro)