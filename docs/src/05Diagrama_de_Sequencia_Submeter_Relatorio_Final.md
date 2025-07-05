### UC.5 - Submeter Relatório Final
```mermaid
sequenceDiagram
    participant Form as EnvioRelatorioFinalForm
    participant Controller as SubmissaoFinalController
    participant FinalReport as RelatorioFinal
    participant Collection as RelatorioCollection

    Form->>Controller: envia(arquivo PDF)
    Controller->>Collection: verificarSubmissaoFinal()
    alt submissão permitida
        Controller->>FinalReport: cria RelatorioFinal
        Controller->>Collection: adicionar(RelatorioFinal)
        Controller-->>Form: confirma envio
    else submissão já realizada
        Controller-->>Form: erro: submissão duplicada
    end