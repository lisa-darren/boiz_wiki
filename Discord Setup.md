
---

# 🎮 Discord Server Setup & Webhooks Configuration

Objective: Transform a standard Discord server into a high-efficiency virtual office for a small dev team.

Principles: Signal-over-noise, automated intelligence, and seamless collaboration.

---

## Phase 1: Server Architecture

Create a clean structure to separate "Deep Work" from "Watercooler" talk.

### 1. Categories & Channels Structure

Delete the default channels and create the following hierarchy:

Plaintext

```
📂 INFORMATION
  #️⃣ announcements       (Milestones, launch dates)
  #️⃣ resources           (Links to Vaultwarden, Nextcloud, Wiki)

📂 DEVELOPMENT
  #️⃣ dev-feed 🤖         (Read-only: GitHub/Figma/Server logs)
  #️⃣ code-chat           (Technical discussions, snippets)
  forums 🐞 bug-tracker   (Forum Channel: Post one thread per bug)

📂 OFFICE
  #️⃣ general             (Casual chat, memes, lunch)
  #️⃣ design-review       (UI/UX feedback)

🔊 VOICE CHANNELS
  🔊 Co-Working (Muted)  (Signal "I am at my desk")
  🔊 Pair Programming 💻 (High bitrate, dedicated for screen sharing)
```

### 2. Channel Specific Settings

**`#dev-feed` (The Intelligence Hub):**

- **Permissions:**
    
    - Right-click channel -> **Edit Channel** -> **Permissions**.
        
    - Select `@everyone`.
        
    - **View Channel:** ✅ Green.
        
    - **Send Messages:** ❌ Red (Prevents humans from cluttering the bot feed).
        

**`🔊 Pair Programming`:**

- **Bitrate:** Right-click channel -> **Edit Channel** -> **Overview**.
    
- **Bitrate Slider:** Maximize it (usually 64kbps or 96kbps) to ensure crisp code text during screen sharing.
    
- **Video Quality:** Ensure "Auto" or "720p" is enabled.
    

---

## Phase 2: Roles & Security

Even for a 3-person team, basic security hygiene is non-negotiable.

1. **Create Roles:**
    
    - Go to **Server Settings** -> **Roles**.
        
    - Create a role named **`@Core Team`** (Pick a distinct color, e.g., Gold).
        
    - Enable **"Administrator"** permission for this role.
        
    - Assign this role to all 3 members.
        
2. **Enforce 2FA:**
    
    - **Policy:** All team members must enable Two-Factor Authentication on their Discord accounts.
        
    - **Server Setting:** Go to **Server Settings** -> **Safety Setup** -> **Moderation** -> Select **Medium** (Must have a verified email).
        

---

## Phase 3: Webhooks Integration (The Automations)

Connect your external tools to post updates automatically into `#dev-feed`.

### A. Setup in Discord (Do this first)

1. Navigate to the **`#dev-feed`** channel.
    
2. Click the **Edit Channel** (gear icon) next to the channel name.
    
3. Select **Integrations** from the left menu.
    
4. Click **Webhooks** -> **New Webhook**.
    
5. **Name:** `GitHub Bot` (or `Ops Bot`).
    
6. **Copy Webhook URL**.
    

### B. Setup in GitHub (Code Push Notifications)

1. Go to your GitHub Repository -> **Settings**.
    
2. Select **Webhooks** on the left sidebar -> **Add webhook**.
    
3. **Payload URL:** Paste the Discord Webhook URL.
    
    - **Important:** Append `/github` to the end of the URL.
        
    - _Example:_ `https://discord.com/api/webhooks/xxxx/xxxx/github`
        
4. **Content type:** Select `application/json`.
    
5. **Which events?** Select **"Just the push event"** (or select individual events like Issues/PRs if preferred).
    
6. Click **Add webhook**.
    

### C. Setup in Figma (Design Updates)

1. Go to your Figma Team page.
    
2. Settings -> **Webhooks** (Note: Figma Webhooks might require a paid plan or a third-party bridge like _Make.com_ or a specific Discord Bot if on the free tier. Alternatively, use the **Figma Discord Bot** available in the App Directory).
    
3. If using a bot: Invite the bot, map it to `#dev-feed`, and link your Figma account.
    

---

## Phase 4: Workflow Best Practices

### 1. Syntax Highlighting

Always use Markdown code blocks when sharing code snippets.

Type this:

```python

def hello():

print("world")

```

**To get this:**

Python

```
def hello():
    print("world")
```

### 2. The "Bug Tracker" Forum

Instead of using Linear or Jira, use the **Forum Channel** (`🐞 bug-tracker`):

- **New Bug:** Create a "New Post". Title = Bug summary. Description = Steps to reproduce.
    
- **Tags:** Create tags for status: `[Backend]`, `[Frontend]`, `[Critical]`.
    
- **Resolved:** Right-click the post -> **Close Post**. It archives the thread cleanly.
    

### 3. Screen Sharing (Pair Programming)

- Hop into the `Pair Programming` voice channel.
    
- Click **"Share Your Screen"**.
    
- **Pro Tip:** Select **"Applications"** -> **VS Code** (instead of "Screens") to stream higher quality text and avoid sharing notification popups.