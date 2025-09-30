# Web Application Assets for Navi Desktop Onboarding

Based on the architecture summary from the ChatGPT conversation, here is a list of every web application asset needed to build for the onboarding and paid conversion flow. Each asset includes a brief description and a text-based wireframe.

## 1. Landing Page (LP)
**Description**: The main marketing page that introduces the app's value proposition, explains the early adopter program, and prompts users to join (since it's a paid app). It sets expectations and qualifies interest.

**Text-Based Wireframe**:
```
+-----------------------------+
|       Header (Logo)         |
+-----------------------------+
|                             |
|   Hero Section              |
|   - App Name & Tagline      |
|   - Value Proposition       |
|   - Early Adopter CTA Button|
|                             |
+-----------------------------+
|                             |
|   Features Section          |
|   - Bullet points - benefits|
|   - Screenshots/Diagrams    |
|                             |
+-----------------------------+
|                             |
|   Footer                    |
|   - Links, Contact          |
+-----------------------------+
```
**Text-Based Wireframe**:
```
+-----------------------------+
|       Header (Logo)         |
|  [Primary CTA]  [Sign in]   |
+----------------------------+
|                             |
|   HERO                     |
|   - App Name & Tagline     |
|     "Navi — Your desktop    |
|      AI co-pilot for work"  |
|   - Value Proposition:     |
|     "Automate workflows,     |
|      unlock creativity, and  |
|   - Primary CTA: [Join Early |
|     Adopters]                |
|                             |
+----------------------------+
|                             |
|   FEATURES & BENEFITS       |
|   - Very Smart & Genius     |
|     agents for daily tasks  |
|   - Automate web workflows  |
|   - Edit & generate images  |
|   - Read & edit local files |
|   - Secure, token-based auth|
|   - Fast local JSON storage  |
|                             |
+----------------------------+
|                             |
|   FOOTER (minimal)          |
|   - © YEAR Navi             |
|   - Terms | Privacy         |
|                             |
+----------------------------+
```

## 2. Native Form (Early Adopter Signup Form)
**Description**: A simple, native web form on the site where users enter their details (name, email, consent) to join the early adopter program. Submits to backend, which saves to Leads table and triggers the download email.

**Text-Based Wireframe**:
```
+-----------------------------+
|       Header (Logo)         |
+-----------------------------+
|                             |
|   Form Title                |
|   "Join Early Adopter Program"|
|                             |
|   Name: [Input Field]       |
|   Email: [Input Field]      |
|   [Consent Checkbox]        |
|                             |
|   [Submit Button]           |
|                             |
+-----------------------------+
|       Footer                |
+-----------------------------+
```


THANK YOU PAGE

    Look at your email to find the access link.

## 4. Create Password Page
**Description**: Accessed via email link when a new user is created or password needs to be set. Allows users to create their initial password, which sets password_empty=false in the backend.

**Text-Based Wireframe**:
```
+-----------------------------+
|       Header (Logo)         |
+-----------------------------+
|                             |
|   Password Creation Title   |
|   "Set Your Password"       |
|                             |
|   Email: [Pre-filled/Read-only] |
|   New Password: [Input Field] |
|   Confirm Password: [Input] |
|                             |
|   [Create Password Button]  |
|                             |
+-----------------------------+
|       Footer                |
+-----------------------------+
```

## 6. Billing/Plans Page
**Description**: A custom page where users select a subscription plan before proceeding to payment. Displays available plans (e.g., monthly, yearly), features, and pricing. Upon selection, redirects to Stripe checkout for payment processing, but the plan selection is hosted on our site for better branding and control.

**Text-Based Wireframe**:
```
+-----------------------------+
|       Header (Logo)         |
+-----------------------------+
|                             |
|   Plans Title               |
|   "Choose Your Plan"        |
|                             |
|   [Pro Plan Card]           |
|   - Name: Pro               |
|   - Price: $15/month        |
|   - Features:               |
|     - Very Smart Agent      |
|     - Automate Web-Based    |
|       Workflows             |
|     - Create and Edit       |
|       Amazing Images        |
|     - Create and Edit Files |
|       on your computer      |
|     - Use Local Files as    |
|       context for your Chat |
|   [Select Pro Plan Button]  |
|                             |
|   [Plus Plan Card]          |
|   - Name: Plus              |
|   - Price: $40/month        |
|   - Features:               |
|     - Everything in Pro +   |
|     - GENIUS Agent          |
|     - Automate even the     |
|       hardest web-based     |
|       workflows             |
|   [Select Plus Plan Button] |
|                             |
|   [Premium Plan Card]       |
|   - Name: Premium           |
|   - Price: $160/month       |
|   - Features:               |
|     - We will whiteglove    |
|       you so that you have  |
|       the best automation   |
|       experience.           |
|     - 24/7 support          |
|   [Select Premium Plan      |
|    Button]                  |
|                             |
+-----------------------------+
|       Footer                |
+-----------------------------+
```

## 7. Success/Confirmation Pages
**Description**: Auxiliary pages for feedback after actions, such as confirming password creation or download initiation. Ensures users know what to do next (e.g., return to app).

**Text-Based Wireframe** (Generic for all success pages):
```
+-----------------------------+
|       Header (Logo)         |
+-----------------------------+
|                             |
|   Success Message           |
|   "Password Created!"       |
|   or "Download Started!"    |
|                             |
|   Next Steps:               |
|   - Return to the app       |
|   - Contact support if issues|
|                             |
|   [Back to App Button]      |
+-----------------------------+
|       Footer                |
+-----------------------------+
```

These assets form the complete web side of the onboarding flow, bridging visitors to paid desktop users. Each is minimalistic and focused on conversion.