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

Flow Summary

1. Discovery & Signup on Web

  - Visitor lands on the marketing page, sees the value proposition and CTA for Early Adopters. Embedded form collects name, email, role and company. Backend validates and stores leads; successful submissions trigger a confirmation UI and a download email.

  - Metrics & expectations: form conversion rate (target 3–10%), server response < 500ms, form validation error rate < 1%.

  - Instrumentation: page_view, form_impression, form_submit, lead_created.

2. Lead Processing & Email Delivery

  - Leads are accepted by the API (POST /leads), deduplicated, and queued for email delivery. Emails contain a download link with a tracking token. Backoff/retry applied for transient email delivery failures.

  - Acceptance: >95% of lead emails delivered within 5 minutes, link click tracked.

3. Download & Install

  - User clicks the download link which routes to a platform-aware download page (Windows/macOS). The download is served from a CDN; installers include telemetry to report successful installation.

  - Instrumentation: download_initiated, download_completed, install_started, install_completed.

4. App Startup & Authentication

  - On first run the app checks for a local token with the backend (verify_token). If valid, user lands in the logged area. If missing or invalid, the app prompts for email and either creates a user from the lead or triggers password setup.

  - Edge cases: expired tokens, clock skew, offline first-run — fall back to email-based flow.

5. Password Setup & Recovery

  - For leads without a password the system sends a secure, single-use password setup link that expires (e.g., 1 hour). The backend enforces rate limits and logs attempts for fraud detection.

  - Acceptance: setup emails sent within 2 minutes and links expire.

6. Paywall & Subscription

  - After authentication, the app checks subscription status. Non-subscribed users see a paywall and can purchase via Stripe (redirect or SDK). Webhooks confirm payment and the backend activates subscriptions atomically.

  - Reliability: webhook processing must be idempotent and retry on transient errors. Paid conversion metric tracked.

7. Instrumentation, Monitoring & Alerts

  - Key metrics to collect: user activation rate, paid conversion, email deliverability, download success rate, login success rate, and error rates. Configure alerts for sustained drops or spike in errors.

8. Edge Cases & Error Handling

  - Handle network outages, partial downloads, duplicate leads, email bounces, and token tampering. Implement retries with exponential backoff and clear user-facing error messages with remediation steps.

9. Acceptance Criteria

  - Mermaid diagram renders correctly on the page.
  - Lead form submissions create leads and trigger download emails.
  - Installer downloads complete and report success events.
  - Auth flows allow password setup and login; token validation works.
  - Payment flow results in activated subscription and measurable conversion metric.