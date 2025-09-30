sequenceDiagram
  autonumber
  actor U as User
  participant W as Website
  participant E as E-mail Service
  participant A as Desktop App
  participant B as Backend/Auth API
  participant S as Billing/Stripe

  %% Discovery & Signup on Web
  U->>W: Visita LP (Landing Page)
  W-->>U: Value prop + CTA Early Adopter
  U->>W: Preenche & envia formulário (nativo)
  W->>B: POST /leads (dados do formulário)
  B-->>W: 200 OK
  B->>E: Enviar e-mail com link de download
  E-->>U: E‑mail: "Download Page"

  %% Download & Install
  U->>W: Abre link de download
  W-->>U: Página de download (Win/macOS)
  U->>U: Baixa e instala o app
  U->>A: Abre o aplicativo

  %% App startup & token check (server-side)
  A->>B: verify_token(local_token)
  alt Token válido
    B-->>A: 200 OK (válido)
    A-->>U: Entra direto (área logada)
  else Sem token / inválido
    B-->>A: 401/invalid
    A-->>U: Solicita e‑mail
    U->>A: Informa e‑mail
    A->>B: lookup_user(email)
    alt Não existe usuário (apenas lead)
      B-->>A: not_found
      A->>B: create_user_from_lead(email)
      B-->>A: created {password_empty:true}
      A->>B: send_password_setup(email)
      B->>E: E-mail "Create Password"
      E-->>U: Link para criar senha
      U->>W: Abre página "Create Password" e define senha
      W->>B: set_password(email, new_password)
      B-->>W: OK
      U->>A: Volta ao app e informa e‑mail
      A->>B: login(email, password)
      B-->>A: token
      A->>A: Salva token (Keychain)
    else Usuário existe
      B-->>A: {password_empty?}
      alt password_empty==true
        A->>B: send_password_setup(email)
        B->>E: E-mail "Create Password"
        E-->>U: Link para criar senha
        U->>W: Define senha
        W->>B: set_password
        B-->>W: OK
        U->>A: Informa e‑mail novamente
        A->>B: login(email, password)
        B-->>A: token
        A->>A: Salva token
      else password_empty==false
        A-->>U: Solicita senha
        U->>A: Digita senha
        A->>B: login(email, password)
        B-->>A: token
        A->>A: Salva token
      end
    end
  end

  %% Paywall / Subscription
  A-->>U: Área logada básica
  A->>B: check_subscription(email)
  alt Já é pago
    B-->>A: subscription=ativa
    A-->>U: Acesso total liberado
  else Não é pago
    B-->>A: subscription=ausente
    A-->>U: Prompt: "Ativar assinatura"
    U->>A: Clica "Ativar"
    A->>S: Seleciona plano (redirect/SDK)
    U->>S: Informa dados de pagamento
    S->>B: Webhook/confirmar pagamento
    B-->>S: OK
    B-->>A: subscription=ativa
    A-->>U: Acesso total liberado
  end