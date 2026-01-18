
---

# 🏗️ Ultra-Lean Geek Stack Architecture

Version: 1.0

Target: 3-Person Technical Team

Philosophy: Maximize personal AI assets, leverage open-source self-hosting, and eliminate "SaaS Tax".

---

## 1. 💰 Financial Overview (Monthly)

This architecture is designed to run on a near-zero budget by utilizing "sunk costs" (existing personal subscriptions) and free-tier developer tools.

| **Component**            | **Provider**              | **Cost (Total)** | **Notes**                                                          |
| ------------------------ | ------------------------- | ---------------- | ------------------------------------------------------------------ |
| **Email Infrastructure** | **Migadu (Micro)**        | **~$4.00**       | The only shared hard cost. Provides `name@company.com`.            |
| **AI Compute**           | **Google One (Personal)** | **$0.00**        | Leveraging existing personal Pro accounts via Identity Separation. |
| **Development IDE**      | **Project IDX**           | **$0.00**        | Cloud-based, includes free AI quota & multiplayer.                 |
| **Comms & Ops**          | **Discord**               | **$0.00**        | Replaces Slack/Zoom.                                               |
| **Secrets Management**   | **Vaultwarden**           | **$0.00**        | Self-hosted. Replaces 1Password Team.                              |
| **Storage & Docs**       | **Nextcloud + Git**       | **$0.00**        | Self-hosted + GitHub Private Repo.                                 |
| **Total**                |                           | **~$4.00 / mo**  |                                                                    |

---

## 2. 🛠️ Technology Stack

### A. Development & AI (The Engine)

- **IDE & Runtime:** **Project IDX** (Google Cloud)
    
    - **Why:** Native browser-based environment, "Multiplayer" cursor collaboration (no setup required), and pre-configured NixOS environments.
        
    - **AI Access Strategy:**
        
        - **Pro Members:** Login to IDX with **Personal Gmail** to unlock full Gemini Advanced ("Antigravity") quota.
            
        - **Free Members:** Utilize IDX's built-in free AI tier or use Gemini Flash via API Key extensions.
            
- **Version Control:** **GitHub** (Free Organization)
    
- **Project Management:** **GitHub Projects**
    
    - **Setup:** Kanban board linked directly to Issues and Pull Requests.
        

### B. Communication (The HQ)

- **Platform:** **Discord**
    
- **Role:** Central Notification Hub & Virtual Office.
    
- **Key Channels:**
    
    - `#dev-feed`: Read-only channel receiving Webhooks (Code pushes, Figma updates, Server alerts).
        
    - `🔊 Pair Programming`: High-bitrate voice channel for screen sharing.
        
- **Search:** Limited. Rely on Obsidian/GitHub for persistent knowledge.
    

### C. Infrastructure (Self-Hosted)

- **Host:** **Docker Container Host** (Managed by System Admin).
    
- **Service 1: Vaultwarden** (Bitwarden Rust Fork)
    
    - **Function:** Enterprise-grade password management.
        
    - **Usage:**
        
        - _Organization Vault:_ Shared AWS keys, Database credentials.
            
        - _Personal Vault:_ Individual user passwords.
            
- **Service 2: Nextcloud**
    
    - **Function:** Archival storage (PDFs, Legal docs, Large assets).
        
    - **Sync:** Desktop clients for file mirroring.
        

### D. Knowledge Management

- **Tool:** **Obsidian** + **Obsidian Git** Plugin.
    
- **Storage:** Private GitHub Repository (`team-wiki`).
    
- **Workflow:** Local Markdown editing -> Auto-commit every 5 mins -> Push to GitHub -> Notify Discord.
    
- **Why:** Zero vendor lock-in, offline access, full version history.
    

### E. Design

- **Tool:** **Figma** (Starter Plan).
    
- **Strategy:** Use the "Unlimited Drafts" folder to bypass the 3-file limit on free teams.
    

---

## 3. 🚦 Operational Workflows (SOPs)

### 🔐 Identity Separation Protocol

To remain compliant while using personal resources for work:

1. **Git Configuration:**
    
    - Configure local Git user email as `name@company.com`.
        
    - _Result:_ Commit history looks professional and corporate.
        
2. **Service Login:**
    
    - Browser/IDE Login: `name@gmail.com` (To access Personal AI/Drive storage).
        

### 🔑 Secret Sharing Protocol

**NEVER** paste plain-text passwords or API Keys into Discord.

1. Open **Vaultwarden**.
    
2. Select the item or text to share.
    
3. Click **"Send"**.
    
4. Configuration:
    
    - _Deletion:_ "After 1 hour" or "After 1 access".
        
5. Paste the generated link into Discord.
    

### 🛡️ Disaster Recovery (Backup)

**Responsibility:** Infrastructure Admin.

- **Automated Script:** Runs daily at 03:00 AM.
    
- **Action:**
    
    1. Dumps Vaultwarden SQLite database.
        
    2. Compresses Nextcloud essential data.
        
    3. Encrypts the archive.
        
    4. Uploads to **Google Drive** (Shared Folder provided by Pro member).
        

---

## 4. Setup Checklist

- [ ] **Email:** Domain configured on Migadu. DNS records (MX, SPF, DKIM) verified.
    
- [ ] **Server:** Vaultwarden deployed via Docker with HTTPS (Nginx/Tunnel).
    
- [ ] **Comms:** Discord Server created with Webhooks connected to GitHub.
    
- [ ] **Wiki:** `team-wiki` Repo created and cloned to all local machines via Obsidian.
    
- [ ] **IDE:** Project IDX workspace initialized and shared with all 3 emails.