# Navi User Journey

This file contains a visual representation of the user journey for investor-facing demos. It includes a Mermaid flowchart you can edit directly in Markdown and an embedded SVG (if your renderer supports it).

## Mermaid Flowchart

```mermaid
flowchart LR
  A[Discovery]\n-- Ads / Content / Referrals --> B[Signup]
  B --> C[Onboarding]\n  C --> D[Automation Build]\n  D --> E[Run & Monitor]\n  E --> F[Iterate / Tune]\n  F --> G[Scale & Upgrade]
  E -->|Insights| C
  click B "./form.html" "Signup form"
  click D "./web_assets.html" "Templates & assets"
```

## SVG

You can view the pre-rendered SVG at `../public/user_journey.svg` which is included in the repo. If you open that file in a browser, you'll see a clean visual workflow designed for investor presentations.

## Notes for embedding

- To embed the SVG into `index.html`, add:

```html
<div style="max-width: 1000px; margin: 2rem auto;">
  <img src="../public/user_journey.svg" alt="Navi user journey" style="width:100%; height:auto; border-radius:12px; box-shadow:0 10px 30px rgba(0,0,0,0.08);" />
</div>
```

- If your Markdown renderer supports Mermaid, the flowchart will render inline. Otherwise, keep the Mermaid source for reference.

---

If you'd like, I can:

- Embed the SVG directly into your `index.html` and commit the change so the Pages site shows the visual.
- Convert the Mermaid flow into a higher-fidelity SVG using a tool and replace or augment `../public/user_journey.svg`.
- Add small animations or step-by-step highlights for interactive demos.

Tell me which option to apply next and I'll update the files and commit.