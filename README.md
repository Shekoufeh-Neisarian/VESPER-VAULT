# VESPER // VAULT — The Asset-Handoff Trap

A spear-phishing awareness project built around a fictional CD PROJEKT RED (CDPR) game-development scenario. A developer receives what appears to be a character-art update from an external studio. The email links to a simulated asset portal, and Gophish records whether the recipient follows the link. The portal then displays a security-awareness message. **No passwords or credentials are collected.**

## Contents

- [Project files](#project-files)
- [Tools and workflow](#tools-and-workflow)
- [Starting the project](#starting-the-project)
- [Security-awareness takeaway](#security-awareness-takeaway)

## Project files

| File | Description |
|---|---|
| [CDPR_Awareness.pptx](presentation/CDPR_Awareness.pptx) | Security-awareness workshop slides |
| [Project report (PDF)](report/VESPER_VAULT_Report.pdf) | Project setup, figures and results |
| [email.html](code/email.html) | Gophish email template with its tracked link |
| [landing.html](code/landing.html) | VESPER // VAULT asset portal and awareness popup |

The repository is organized into `presentation/`, `report/` and `code/` folders. **The links above work when the names and capitalization match the files uploaded to GitHub.**

## Tools and workflow

**Gophish** manages the campaign, email template, recipient group, landing page and link-click results. **SMTP** (port `1025`) carries the messages to **MailHog**, which stores them and displays them in a browser on port `8025`.

**Flow:** Gophish → SMTP (`1025`) → MailHog (`8025`) → email link → landing page (`8080`) → Gophish results (`3333`).

In `email.html`, `{{.URL}}` is replaced by Gophish with the recipient-specific campaign link. Clicking **OPEN ASSET REVIEW** opens the landing page and records a click. Clicking **REQUEST ASSET ACCESS** on the landing page reveals the awareness message; this second button is not a separate Gophish click-tracking event.

## Starting the project

These commands run **inside Kali Linux** with Gophish and the MailHog Docker container already installed.

**Terminal 1 — MailHog**

```bash
sudo systemctl start docker
sudo docker start cdpr-mailhog
sudo docker ps
```

**Terminal 2 — Gophish**

```bash
cd ~/CDPR-gophish
./gophish
```

Open the **Gophish admin dashboard** at `https://127.0.0.1:3333` (or `http://` if TLS is disabled) and the **MailHog inbox** at `http://127.0.0.1:8025`, both inside Kali.

The prepared campaign is named **CDPR Demo**. Open its results in Gophish to review previous events, or create a new campaign with the existing settings for a fresh demonstration.

## Security-awareness takeaway

Game developers often receive files and review requests from external partners. Before opening unexpected asset-sharing links, verify the request and platform through an established project channel. Report suspicious messages or unexpected login requests promptly.
