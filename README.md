# VESPER // VAULT — The Asset-Handoff Trap

A spear-phishing awareness project based on a fictional CD PROJEKT RED (CDPR) game-development scenario. An external art partner appears to share updated character concepts with a game developer. The email leads to a simulated asset-review portal, and Gophish records whether a recipient clicks the email link. The portal then reveals the security-awareness message; **no passwords or credentials are collected**.

## Project structure

```text
VESPER-VAULT/
├── README.md
├── presentation/
│   └── CDPR_Awareness.pptx
├── report/
│   └── VESPER_VAULT_Report.pdf
└── code/
    ├── email.html            # Gophish email template
    └── landing.html          # Asset portal and awareness popup
```

*The report filename is an example; keep the actual name of your PDF when uploading.*

## Tools and workflow

- **Gophish** creates the campaign, sends test emails, serves the landing page, and tracks link clicks.
- **SMTP** delivers emails from Gophish to **MailHog**, which stores them in a test inbox.
- **Frontend:** `email.html` contains the `{{.URL}}` tracking link; `landing.html` displays the asset portal and awareness message.

**Flow:** Gophish → SMTP (`1025`) → MailHog inbox (`8025`) → email link → Gophish landing page (`8080`) → campaign results (`3333`).

## Run the project (Kali Linux)

The following assumes Gophish and the `cdpr-mailhog` Docker container are already installed.

**Terminal 1 — MailHog:**

```bash
sudo systemctl start docker
sudo docker start cdpr-mailhog
sudo docker ps
```

**Terminal 2 — Gophish:**

```bash
cd ~/CDPR-gophish
./gophish
```

Open these pages **inside Kali**:

- **Gophish admin:** `https://127.0.0.1:3333` (or `http://` if TLS is disabled in `config.json`)
- **MailHog inbox:** `http://127.0.0.1:8025`

To show the full flow, open an email in MailHog and click **OPEN ASSET REVIEW**. The link opens the VESPER // VAULT landing page and Gophish records the click. On the landing page, **REQUEST ASSET ACCESS** displays the awareness popup; this second button is not a separate Gophish click-tracking event.

The prepared campaign is named **CDPR Demo**. Its prior results can be reviewed directly; a new campaign is needed only to demonstrate fresh recipient statistics.

## Awareness takeaway

Before opening an unexpected asset-sharing link, game developers should verify the sender and the sharing platform through an established project channel. If a link seems suspicious or has already been opened, report it promptly to the security team.
