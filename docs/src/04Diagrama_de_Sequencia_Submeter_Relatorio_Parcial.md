### UC.4 - Submeter Relatório Parcial
```mermaid
sequenceDiagram
    participant Form as EnvioRelatorioForm
    participant Controller as SubmissaoController
    participant Report as Relatorio
    participant Collection as RelatorioCollection

    Form->>Controller: envia(arquivo PDF)
    Controller->>Report: instancia novo Relatorio
    Controller->>Collection: validarDuplicidade(Relatorio)
    alt não duplicado
        Controller->>Collection: adicionar(Relatorio)
    else duplicado
        Controller-->>Form: feedback de erro
    end