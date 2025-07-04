# Diagramas de Classes de Análise (Mermaid)

Este documento contém os diagramas de classes de análise para os casos de uso do Sistema ConectaAE, utilizando a sintaxe Mermaid para visualização direta no GitHub.

## UC.1 - Logar

```mermaid
classDiagram
    direction LR
    class "TelaLogin" as TL <<boundary>>
    class "ControleLogin" as CL <<control>>
    class "Usuario" as U <<entity>>
    class "RepositorioUsuarios" as RU <<entity collection>>

    TL -- CL : usa
    CL -- RU : usa
    RU -- U : gerencia
    CL ..> U : colabora
