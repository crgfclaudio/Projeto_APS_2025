# Diagramas de Classes de Análise (Mermaid)

Este documento contém os diagramas de classes de análise para os casos de uso do Sistema ConectaAE, utilizando a sintaxe Mermaid para visualização direta no GitHub.

## UC.1 - Logar

%%{ init : { "theme" : "default" } }%%
classDiagram
    class "TelaLogin" as TL <<boundary>>
    class "ControleLogin" as CL <<control>>
    class "Usuario" as U <<entity>>
    class "RepositorioUsuarios" as RU <<collection of entities>>

    TL -- CL : EUA
    CL -- RU : eua
    RU -- U : gerencia
    CL ..> U : colabora
