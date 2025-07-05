### UC.1 - Logar
```mermaid
sequenceDiagram
    actor Cliente

    participant LoginUI              as LoginUI (Boundary)
    participant AuthController       as AuthController (Control)
    participant User                 as User (Entity)
    participant Sessions             as Sessions (EntityCollection)

    Cliente->>LoginUI: login(credentials)
    LoginUI->>AuthController: login(credentials)

    AuthController->>User: findUser(credentials.username)
    User-->>AuthController: usuárioEncontrado

    AuthController->>User: verifyPassword(credentials.password)
    User-->>AuthController: senhaVálida?

    alt senhaVálida
        AuthController->>Sessions: createSession(usuário)
        Sessions-->>AuthController: sessãoCriada
        AuthController-->>LoginUI: mostrarDashboard(sessãoCriada)
    else senhaInválida
        AuthController-->>LoginUI: mostrarErro("Credenciais inválidas")
    end

