```mermaid
sequenceDiagram
  participant Boundary as LoginUI (Boundary)
  participant Control as AuthController (Control)
  participant Entity as User (Entity)
  participant EntityCollection as Sessions (EntityCollection)

  Boundary->>Control: login(credentials)
  Control->>Entity: findUser(credentials.username)
  Entity-->>Control: user
  Control->>Entity: verifyPassword(credentials.password)
  Entity-->>Control: validPassword?
  alt validPassword
    Control->>EntityCollection: createSession(user)
    EntityCollection-->>Control: session
    Control-->>Boundary: showDashboard(session)
  else invalidPassword
    Control-->>Boundary: showError("Credenciais inválidas")
  end
