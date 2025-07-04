# Diagramas de Classes de Análise (Mermaid)

## UC.1 - Logar (Organizado para Requisitos de Análise)

```mermaid
%%{ init : { "theme" : "default" } }%%
classDiagram
    direction LR
    class "TelaLogin" as TL <<boundary>>
    class "ControleLogin" as CL <<control>>
    class "Usuario" as U <<entity>>
    class "RepositorioUsuarios" as RU <<collection of entities>>

    TL -- CL : EUA
    CL -- RU : eua
    RU -- U : gerencia
    CL ..> U : colabora
