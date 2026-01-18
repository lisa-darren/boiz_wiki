# 📘 Obsidian + Git Sync Workflow

**Objective:** collaborative knowledge management with version control, zero cost, and seamless Discord notifications.

**Prerequisites:**

- [Git](https://git-scm.com/) installed on your local machine.
    
- [Obsidian](https://obsidian.md/) installed.
    
- A GitHub account.
    

---

## Phase 1: Repository Setup (Admin Only)

Role: Team Lead / Administrator

Goal: Create the "Source of Truth" in the cloud.

1. **Create Repository:**
    
    - Log in to GitHub.
        
    - Create a new **Private** repository (e.g., `team-wiki` or `company-knowledge`).
        
    - **Crucial:** Check the box **"Add a README file"** (this initializes the `main` branch).
        
2. **Get URL:**
    
    - Click the green **Code** button.
        
    - Copy the **SSH** (recommended) or **HTTPS** URL.
        

---

## Phase 2: Local Initialization (All Members)

Role: Every Team Member

Goal: Clone the knowledge base to the local machine.

1. Clone the Repo:
    
    Open your terminal (Terminal, PowerShell, or Git Bash) and navigate to your preferred document folder. Run:
    
    Bash
    
    ```
    git clone git@github.com:YourOrg/team-wiki.git
    ```
    
2. **Open in Obsidian:**
    
    - Launch Obsidian.
        
    - Click **"Open folder as vault"**.
        
    - Select the newly cloned `team-wiki` folder.
        

---

## Phase 3: Plugin Configuration (The Magic)

Role: Every Team Member

Goal: Automate the Sync process.

1. **Install Plugin:**
    
    - Go to **Settings** -> **Community plugins**.
        
    - Turn off "Safe mode" if prompted.
        
    - Click **Browse** and search for **"Obsidian Git"** (by Vinzent).
        
    - **Install** and **Enable** the plugin.
        
2. Configure Settings:
    
    Find "Obsidian Git" in the plugin list and apply these settings:
    
    - **Vault backup interval (minutes):** Set to `5` (or `10`).
        
        - _Effect:_ Automatically commits and pushes changes every 5 minutes.
            
    - **Auto backup after file change:** Toggle **ON** (optional, for faster sync).
        
    - **Pull updates on startup:** Toggle **ON** (Critical!).
        
        - _Effect:_ Pulls the latest changes from teammates immediately when you open Obsidian.
            
    - **Commit message:** Leave default or set to `{{date}} {{time}} - {{hostname}}`.
        

---

## Phase 4: Ignore Rules (.gitignore)

Role: Admin (Do this once)

Goal: Prevent UI/Layout conflicts between members.

Obsidian stores your personal window layout and theme settings in a hidden folder. We want to sync the _content_, not your _window layout_.

1. Create a file named `.gitignore` in the root of the `team-wiki` folder.
    
2. Paste the following content:
    

代码段

```
# System files
.DS_Store
Thumbs.db

# Obsidian Workspace (Personal Layouts)
.obsidian/workspace
.obsidian/workspace.json
.obsidian/workspace-mobile.json

# (Optional) Uncomment below if members want different plugins
# .obsidian/plugins/

# (Optional) Uncomment below if members want different themes
# .obsidian/themes/

# Cache
.obsidian/cache
```

3. **Push the ignore file:**
    
    Bash
    
    ```
    git add .gitignore
    git commit -m "chore: configure gitignore"
    git push
    ```
    

---

## Phase 5: Discord Notification Integration

**Goal:** Get a "ping" in Discord whenever knowledge is updated.

1. **Discord Side:**
    
    - Go to your `#dev-feed` channel settings -> **Integrations** -> **Webhooks**.
        
    - Create a new Webhook and copy the **Webhook URL**.
        
2. **GitHub Side:**
    
    - Go to the `team-wiki` repository -> **Settings** -> **Webhooks** -> **Add webhook**.
        
    - **Payload URL:** Paste the Discord URL and append `/github` at the end.
        
        - _Example:_ `https://discord.com/api/webhooks/xxxx/xxxx/github`
            
    - **Content type:** Select `application/json`.
        
    - **Events:** Select **"Just the push event"**.
        
    - Click **Add webhook**.
        

---

## Phase 6: Conflict Resolution SOP

Scenario: Two members edit the same line in the same file at the same time.

Result: Obsidian Git reports a "Merge Conflict".

**How to fix:**

1. **Do not** try to fix it inside Obsidian (it might look messy).
    
2. Open the `team-wiki` folder in **VS Code**.
    
3. VS Code will highlight the conflict areas:
    
    - `<<<< HEAD` (Current changes on your machine).
        
    - `>>>> Incoming` (Changes from the server).
        
4. Click **"Accept Both"** or choose the correct version.
    
5. Save the file.
    
6. Open the terminal in VS Code and run:
    
    Bash
    
    ```
    git add .
    git commit -m "fix: resolve merge conflict"
    git push
    ```
    

---

## Summary Checklist

- [ ] Repository created with README.
    
- [ ] Repo cloned locally.
    
- [ ] Obsidian Git plugin installed.
    
- [ ] **"Pull on startup"** is enabled.
    
- [ ] `.gitignore` is configured and pushed.
    
- [ ] Discord Webhook is active.