# Navi Desktop App Architecture and Onboarding Summary

Based on the ChatGPT conversation accessed on September 25, 2025, here's a comprehensive summary of the discussion on the CrewAI Desktop app architecture and onboarding flow. This appears to be the "App architecture overview" referenced, focusing on a Python-based desktop application with AI agents, web onboarding, and paid conversion.

## 1. App Overview and Core Features
- **Purpose**: A desktop app built in Python, using Navi (a framework for AI agents) with one Personal Assistant Agent running in verbose mode. It integrates MCP (Model Context Protocol) for tools, specifically Playwright MCP for headed browser navigation (Chrome browser setup).
- **LLM**: OpenAI GPT-5-nano.
- **UI**: ChatGPT-like interface with a sidebar containing a "Chat" list (chat history) and a "Tasks" button. The main screen shows user-agent messages, agent/tool execution logs (inline system messages, no separate log component), and verbose logs.
- **Workspace**: JSON files for local persistence (speed, safety, manageability). Folders: one for chats (per-chat JSON) and one for tasks (each with Objective, Description, and Deliverable).
- **Build**: Desktop app for Windows and macOS (not web). On first message, the chat displays the message, and the agent thinks/runs in verbose mode.
- **Tasks Feature**: Initially included but later removed to focus on chat + MCP. If re-added, users could view/run/create/edit tasks, with the chat contracting to the right (like Cursor/Windsurf view).
- **Initial Spec Evolution**: Started with task management, custom tools, and DALL·E for forms, but simplified to chat-only, removed tasks, and switched to native web forms.

## 2. Onboarding and Paid Conversion Flow
- **High-Level Flow**: Visitors → Paid Users via a modern, seamless web-to-desktop journey. Emphasizes marketing/product blend, with clear expectations (paid app, early adopter program).
- **Key Principles**:
  - Separate **Leads table** (from web form) and **Users table** (created only after desktop app email input).
  - Backend/Auth API handles emails (download link, password creation), not the website.
  - Token verification always on backend (not local).
  - Local persistence: Token and last email (secure storage like Keychain/OS); never store passwords.
  - Security: Emails from backend, no local token validation.

- **Detailed Steps** (Web → Desktop):
  1. **Landing Page (Web)**: Marketing site with value prop, early adopter CTA, and paid app clarity.
  2. **Form Submission (Web → Backend)**: Native form (name, email, consent) → Backend saves to Leads table → Returns OK to site.
  3. **Email with Download Link (Backend → Email)**: Backend sends email with download page link (Win/macOS buttons).
  4. **Download & Install (Desktop)**: User downloads/installs app.
  5. **Token Verification (Desktop → Backend)**: App checks local token; if invalid/missing, prompts for email.
  6. **Email/Password Flow (Desktop + Backend + Email + Web)**:
     - If no user (lead only): Backend creates user from lead (password_empty=true), sends password creation email.
     - If user exists but password_empty=true: Resends password creation email.
     - If active user: Prompts for password in app.
  7. **Login (Desktop → Backend)**: Email + password → Backend issues token → App authenticates.
  8. **Persistence (Desktop)**: Saves token/last email securely.
  9. **Paid Gate (Desktop)**: If not paid, prompts to activate subscription → Billing page → Stripe payment → Full features.

- **Visual Diagrams**:
  - **ASCII Flow**: Web (LP → Form → Backend saves lead → Email download link → Download page → Download app) → Desktop (App start → Verify token → Email input → User exists? → Password empty? → Password input → Logged area → Paid? → Subscription prompt → Billing → Payment → Full features).
  - **Mermaid BPMN/Userflow**: Swimlanes for Website, Email, Desktop App, Billing/Stripe. Includes gates like "User account active?" and "Paid user?".
  - **Sequence Diagram**: Actors: User, Website, Email Service, Desktop App, Backend/Auth API, Billing/Stripe. Covers signup, download, token check, user creation, login, and payment.

- **Pseudocode for Auth**:
  ```python
  # on app start
  local_token = storage.read("auth_token")
  if local_token:
      if not api.verify_token(local_token):  # backend verifies
          local_token = None

  if not local_token:
      email = ui.prompt_email()
      user = api.lookup_user(email)
      if not user:
          # creates user from lead and sends email for password creation
          api.create_user_from_lead_and_send_setup(email)
          ui.info("We sent a link to create your password. Return to the app after completing.")
          return
      if user.password_empty:
          api.send_password_setup(email)
          ui.info("We sent a link to create your password. Return to the app after completing.")
          return
      password = ui.prompt_password()
      token = api.login(email, password)
      storage.save_secure("auth_token", token)
      storage.save("last_email", email)
  else:
      token = local_token

  app.session = token
  ui.proceed_to_main()
  ```

## 3. Web Assets to Build
- **Landing Page**: Marketing page with early adopter CTA.
- **Native Form**: Simple form (name, email, consent) → Leads table.
- **Download Page**: Accessed via email link; buttons for Win/macOS downloads.
- **Create Password Page**: Via email link; user sets initial password.
- **Success/Confirmation Pages**: E.g., "Password created—return to app," "Download started."
- **Rationale**: Minimal, consistent with LP branding; no DALL·E or Tally.

## 4. Technical Architecture
- **Navi Integration**: 1 agent with MCP tools (Playwright for browser); verbose mode for logs.
- **MCP Wiring**: Only Playwright MCP (stdio); custom tools local (not MCP).
- **Data Flow**: Website → Backend (leads); App → Backend (auth, token); Emails from Backend.
- **Packaging**: Python app with PySide6 UI skeleton (sidebar, chat view, tasks view if added).
- **Security**: Backend token validation; secure local storage; no password persistence.

## 5. Evolution and Corrections
- **Initial Draft**: Included tasks, DALL·E forms, local token checks—removed/simplified.
- **Corrections**: Form via native web (not DALL·E); backend sends download email; token always backend-verified; users table only after app email; leads separate.
- **Open Questions**: Export to BPMN 2.0 XML, .drawio, or .plantuml for diagrams; add PySide6 signals/slots; minimal custom_mcp_server.py stub.

This summary captures the iterative refinement of the app's PRD, architecture, and onboarding. If you need to incorporate this into the navi-web repository (e.g., for the web assets or flow diagrams), let me know how to proceed—such as creating files or planning the implementation.