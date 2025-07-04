### UC.21 - Cadastrar por Lote (Administrador)

```mermaid
%% Diagrama de Classe para UC.21 - Cadastrar por Lote
classDiagram
    class PainelAdministrador {
        <<boundary>>
        +enviarArquivoCSV()
        +validarRegistros()
        +confirmarImportacao()
    }

    class LoteCadastroController {
        <<control>>
        +validarArquivo()
        +importarUsuarios()
    }

    class Usuario {
        <<entity>>
        +nome: String
        +email: String
        +cpf: String
        +curso: String
    }

    class UsuarioCollection {
        <<entity collection>>
        +verificarDuplicidades()
        +adicionarEmLote()
    }

    PainelAdministrador --> LoteCadastroController : envia CSV
    LoteCadastroController --> Usuario : instancia
    LoteCadastroController --> UsuarioCollection : armazena
