# Getting Started with Your Productivity HQ

Welcome to your AI-powered productivity workspace. This guide will walk you through setup step-by-step.

**How to use this guide:** Ask Claude: *"Walk me through GETTING-STARTED.md"*

Claude will guide you through each step and verify your setup at checkpoints.

---

## What You're Building

Your personal AI productivity headquarters that can:
- Read your calendar and prep you for meetings
- Triage your inbox and draft responses
- Aggregate daily priorities into one briefing
- Execute workflows across your tools

---

## Quick Glossary

New to this? Here's what the technical terms mean:

| Term | What it means |
|------|---------------|
| **MCP** | Model Context Protocol - how Claude connects to other apps (like Gmail) |
| **OAuth** | The secure login system Google uses - you'll see "Sign in with Google" |
| **API** | How apps talk to each other - you're giving Claude permission to talk to Google |
| **Credentials** | A special file (like a password) that proves you own this Google connection |
| **Terminal** | The text-based command window (Command Prompt on Windows, Terminal on Mac) |

---

## Prerequisites

Before starting, you need:

### IDE (pick one)

Any VSCode-based IDE works:
- [VSCode](https://code.visualstudio.com/download)
- [Cursor](https://cursor.com)
- [Windsurf](https://windsurf.ai)
- [Google Antigravity](https://antigravity.google)

### Claude Code

| Tool | Link |
|------|------|
| **Claude Code CLI** | [Install Guide](https://docs.anthropic.com/en/docs/claude-code) |
| **Claude Code Extension** | [VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=anthropic.claude-code) (recommended) |

### Other

- **Google Account** - For Gmail and Calendar access via MCP

---

## Step 0: Install Claude Code Extension (Recommended)

The Claude Code extension gives you an integrated experience inside your IDE.

1. Open your IDE (VSCode, Cursor, Windsurf, etc.)
2. Go to Extensions (Cmd+Shift+X on Mac, Ctrl+Shift+X on Windows)
3. Search for "Claude Code"
4. Install the extension by Anthropic

**Features you get:**
- Chat panel integrated in your IDE
- Checkpoints - press `Esc` twice to rewind changes
- File context awareness

**Alternative:** You can also run `claude` directly in your terminal.

### Checkpoint

You should see the Claude Code icon in your IDE sidebar.

- [ ] **PASS:** Claude Code extension is installed

---

## Step 1: Configure Your Identity

Open `CLAUDE.md` and fill in the Configuration section:

```markdown
| Setting | Value |
|---------|-------|
| **Name** | [Your Full Name] |
| **Email** | [your-email@domain.com] |
| **Timezone** | [America/New_York] |
```

**Find your timezone:** https://en.wikipedia.org/wiki/List_of_tz_database_time_zones

### Checkpoint

Ask Claude: *"What's my name and timezone?"*

Claude should respond with the values you just entered.

- [ ] **PASS:** Claude knows my name and timezone

**Troubleshooting:**
- Make sure you saved CLAUDE.md after editing
- Verify CLAUDE.md is in the workspace root (not in a subfolder)
- Try restarting Claude Code

---

## Step 2: Connect Your Tools

Claude can connect to your calendar, email, and files. Let's set up what you need.

### What Each Integration Does

| Integration | What Claude Can Do | Example Uses |
|-------------|-------------------|--------------|
| **Google Calendar** | See your events, create meetings, check availability | "What's on my calendar today?", "Schedule a call with John" |
| **Gmail** | Read emails, draft replies, search messages | "Any urgent emails?", "Draft a reply to Sarah's message" |
| **Google Drive** | Read documents, find files | "What's in the Q4 report?", "Find the contract PDF" |

**You can start with just one** and add more later. Calendar is the most useful for daily briefings.

---

### What do you want Claude to access?

**Tell Claude:** "I want to set up [your choice]"

| If you want... | Tell Claude... |
|----------------|----------------|
| Google Calendar + Gmail | "Set up Google Workspace" |
| Only Google Calendar | "Set up just Google Calendar" |
| Only Gmail | "Set up just Gmail" |
| Microsoft Outlook + Calendar | "Set up Microsoft 365" *(coming soon)* |
| Both Google and Microsoft | "Set up Google first, then Microsoft" |

Claude will guide you through the specific steps for your choice.

---

## Option A: Google Workspace Setup

*Follow this section if you chose Google Calendar, Gmail, or both.*

**🚨 IMPORTANT: Claude Code vs Claude Desktop**

This guide is for **Claude Code** (the CLI tool and VS Code extension), NOT the Claude Desktop app.

| If you're using... | You're in the right place? |
|--------------------|---------------------------|
| VS Code/Cursor/Windsurf with Claude extension | ✅ Yes |
| Terminal with `claude` command | ✅ Yes |
| Claude Desktop app (standalone Mac/Windows app) | ❌ No - different setup needed |

**How to check:** If you're reading this in your IDE's sidebar or ran `/daily` to get here, you're using Claude Code. ✅

---

### Before You Start

**What you'll need:**
- A Google account (personal Gmail or Google Workspace)
- Your `credentials.json` file (you'll create this below)

**Two consoles to know about:**

| Console | URL | Purpose |
|---------|-----|---------|
| Google Cloud Console | [console.cloud.google.com](https://console.cloud.google.com/) | Create OAuth credentials (what we'll use) |
| Google Admin Console | [admin.google.com](https://admin.google.com/) | Only for Google Workspace admins |

**Personal Gmail users:** Use Google Cloud Console (no admin access needed).

**Google Workspace users:** You may need your IT admin to approve the app, or use a personal Gmail for testing.

---

### 2.1 Create Your Google Cloud Project

**Go to:** [Google Cloud Console](https://console.cloud.google.com/)

Sign in with the Google account you want Claude to access.

#### Create a New Project

1. Look at the top left, next to the "Google Cloud" logo
2. Click the **project dropdown** (might say "Select a project" or show a project name)
3. In the popup, click **New Project** (top right corner of the popup)
4. Enter project details:
   - **Project name:** `Claude MCP` (or any name you like)
   - **Organization:** Leave as default (or "No organization")
5. Click the blue **Create** button
6. Wait a few seconds for the project to be created
7. Click **Select Project** when prompted, or select it from the dropdown

**✓ You'll know it worked when:**
- You see "Claude MCP" (or your project name) in the top dropdown
- The page shows your project dashboard

---

### 2.2 Enable the APIs

Your project needs permission to access Gmail, Calendar, etc.

1. In the left sidebar, look for **APIs & Services** (you may need to click the hamburger menu ☰)
2. Click **Library** (or go directly to [API Library](https://console.cloud.google.com/apis/library))
3. Enable each API you need:

**Gmail API:**
1. In the search box, type "Gmail API"
2. Click on **Gmail API** in the results
3. Click the blue **Enable** button
4. Wait for it to enable (you'll see a dashboard)

**✓ You'll know it worked when:** The button changes from "Enable" to "Manage"

**Google Calendar API:**
1. Click **Library** in the left sidebar to go back
2. Search for "Google Calendar API"
3. Click on **Google Calendar API**
4. Click **Enable**

**Google Drive API (optional):**
1. Search for "Google Drive API"
2. Click **Enable** (gives Claude access to your Drive files)

**✓ You'll know it worked when:**
- Go to **APIs & Services** → **Enabled APIs**
- You see Gmail API and Google Calendar API in the list

---

### 2.3 Configure OAuth Consent Screen

This tells Google what your app is and who can use it.

1. In the left sidebar, click **APIs & Services** → **OAuth consent screen**
2. You'll be asked to select **User Type:**
   - **External** - Choose this (works for any Google account)
   - **Internal** - Only shows if you have Google Workspace (limits to your org)
3. Click **Create**

**Fill in App Information:**

| Field | What to Enter |
|-------|---------------|
| **App name** | `Claude MCP` |
| **User support email** | Your email address (select from dropdown) |
| **App logo** | Skip (optional) |
| **App domain** | Skip all of these |
| **Developer contact email** | Your email address |

4. Click **Save and Continue**

**Add Scopes (Permissions):**

1. Click **Add or Remove Scopes**
2. In the search box or filter, find and check these scopes:

| Scope | What it allows |
|-------|----------------|
| `https://www.googleapis.com/auth/gmail.modify` | Read/write emails |
| `https://www.googleapis.com/auth/gmail.send` | Send emails |
| `https://www.googleapis.com/auth/calendar` | Full calendar access |
| `https://www.googleapis.com/auth/calendar.events` | Create/edit events |
| `https://www.googleapis.com/auth/drive.readonly` | Read Drive files (optional) |

3. Click **Update**
4. Click **Save and Continue**

**Add Test Users:**

While your app is in "Testing" mode, only these users can authenticate.

1. Click **Add Users**
2. Enter your Google email address
3. Click **Add**
4. Click **Save and Continue**

**Review and Finish:**
1. Review your settings
2. Click **Back to Dashboard**

**✓ You'll know it worked when:**
- OAuth consent screen shows "Testing" status
- Your app name "Claude MCP" appears

---

### 2.4 Create Your Login Credentials

This step creates a special file that lets Claude log into Google on your behalf.

**What you're doing:** Creating a secure "key" that only you and Claude will have.

#### Step-by-Step:

1. **Find the Credentials page**
   - In the left sidebar, look for **APIs & Services**
   - Click **Credentials**

2. **Create new credentials**
   - At the top of the page, find the blue **+ Create Credentials** button
   - Click it
   - A dropdown menu appears - click **OAuth client ID**

3. **If asked about consent screen:**
   - You might see: "To create an OAuth client ID, you must first configure your consent screen"
   - If so, click **Configure Consent Screen** and follow section 2.3 first
   - Then come back here

4. **Configure the client**
   - **Application type:** Click the dropdown and select **Desktop app**
   - **Name:** Type `Claude MCP Client` (or any name you'll remember)
   - Click the blue **Create** button

5. **A popup appears** showing your Client ID and Client Secret
   - Click **OK** to close it for now

6. **Add the redirect URL** (IMPORTANT - don't skip this!)
   - Find your new client in the OAuth 2.0 Client IDs list
   - Click on the client name to open its settings
   - Scroll down to **Authorized redirect URIs**
   - Click **+ Add URI**
   - Type exactly: `http://localhost:8000/oauth2callback`
   - Click **Save**

   **⚠️ Why this matters:** When you log in, Google needs to know where to send you back. Without this URL, you'll see "localhost refused to connect."

7. **Download your credentials file**
   - Go back to the Credentials page (click Credentials in the breadcrumb)
   - Find your OAuth client in the "OAuth 2.0 Client IDs" list
   - On the right side, click the download icon (⬇️ arrow)
   - Save the file - it will be named something like `client_secret_xxxxx.json`
   - **Rename it to:** `credentials.json`

**✓ You'll know it worked when:**
- You have a file called `credentials.json` in your Downloads folder
- The file is about 1KB in size
- If you open it in a text editor, you'll see `"client_id"` and `"client_secret"` inside

---

### 2.5 Store and Configure Your Credentials

Now connect your credentials to Claude Code.

#### Store Credentials Securely

Move your credentials to a permanent location (not Downloads):

**Mac/Linux:**
```bash
# Create a folder for MCP credentials
mkdir -p ~/.mcp-credentials

# Move your credentials file
mv ~/Downloads/credentials.json ~/.mcp-credentials/google-credentials.json
```

**Windows (PowerShell):**
```powershell
# Create a folder for MCP credentials
mkdir $env:USERPROFILE\.mcp-credentials

# Move your credentials file
move $env:USERPROFILE\Downloads\credentials.json $env:USERPROFILE\.mcp-credentials\google-credentials.json
```

#### Where Claude Code Stores MCP Settings

Claude Code's MCP settings are stored in a file called `settings.local.json`:

**Config file location:**
- **Mac/Linux:** `~/.claude/settings.local.json`
- **Windows:** `C:\Users\YourName\.claude\settings.local.json`

**⚠️ NOT the Claude Desktop config at:**
- Mac: `~/Library/Application Support/Claude/claude_desktop_config.json`
- Windows: `%APPDATA%\Claude\claude_desktop_config.json`

The MCP server installation (next step) will configure this for you automatically.

**✓ You'll know it worked when:**
- Your credentials.json is stored in `~/.mcp-credentials/` (not Downloads)

---

### 2.6 Install the MCP Server

Now install the MCP server that will use your credentials.

**Recommended:** [google_workspace_mcp](https://github.com/taylorwilsdon/google_workspace_mcp) - Most feature-complete

**Option A: Smithery CLI (Easiest)**
```bash
npx -y @smithery/cli install mcp-gsuite --client claude
```
Then follow prompts to add your `credentials.json` path.

**Option B: Manual Setup**
```bash
# Clone the repo
git clone https://github.com/taylorwilsdon/google_workspace_mcp.git
cd google_workspace_mcp

# Install dependencies
npm install

# Copy your credentials.json to the project folder
cp ~/.mcp-credentials/google-credentials.json ./credentials.json

# Run authentication
npm run auth
```

See the [MCP Servers Directory](https://github.com/modelcontextprotocol/servers) for other options.

**✓ You'll know it worked when:**
- The installation completes without errors
- You see the MCP server listed in your Claude settings

---

### 2.7 Log In to Google (Authentication)

This is where you actually connect Claude to your Google account.

**What happens:** A browser window opens, you log into Google, and Claude saves the connection.

**⚠️ Common Mistake:** Don't close the terminal window while authenticating! The process needs to stay running until you see "Success."

#### Step-by-Step:

1. **Start the authentication**
   - Open your terminal
   - Navigate to where you installed the MCP server
   - Run the authentication command (usually `npm run auth`)

2. **A browser window opens automatically**
   - If it doesn't open, look in your terminal for a URL to copy/paste

3. **Sign in to Google**
   - Use the **same Google account** you added as a test user earlier
   - If you see "This app isn't verified" - that's normal! Click **Continue**
   - Review the permissions and click **Allow**

4. **Return to your terminal**
   - You should see: "Authentication successful!" or similar
   - **Keep the terminal open** until you see this message

**✓ You'll know it worked when:**
- Terminal shows "Authentication successful" or "Credentials saved"
- A new file appears (usually called `token.json` or similar)

---

### 2.8 Update CLAUDE.md

Now tell Claude about your Google connection.

Open `CLAUDE.md` and uncomment/fill in the Google Workspace section:

```markdown
### Google Workspace (Gmail, Calendar, Drive)

| Parameter | Value |
|-----------|-------|
| `user_google_email` | your-actual-email@gmail.com |

**Verification:** Ask "What's on my calendar today?"
```

### Checkpoint

Ask Claude: *"What's on my calendar today?"*

Claude should query your Google Calendar and show your events.

- [ ] **PASS:** Claude can see my calendar events

**Troubleshooting:**
- If authentication failed, re-run the setup and grant all permissions
- If no events show, check you have events scheduled for today
- Verify the email in CLAUDE.md matches your Google account

---

## Option B: Microsoft 365 Setup (Coming Soon)

*This section is under development. Join the community for updates.*

Microsoft 365 connection will include:
- Outlook Calendar
- Outlook Email
- OneDrive files

**Want this sooner?** Let us know in the community: [theoperationsguide.com/community](https://theoperationsguide.com/community)

---

## Step 3: Run Your First Briefing

Now let's test the `/daily` command.

### 3.1 Update the Command

Open `.claude/commands/daily.md` and update:
- Line 10: Replace `[your-email@domain.com]` with your actual email
- Line 11: Replace `[America/New_York]` with your timezone

### 3.2 Run It

In Claude Code, type:

```
/daily
```

### Checkpoint

You should see a formatted briefing with:
- TOP 5 ACTIONS (prioritized list)
- Today's Schedule (your calendar events)
- Emails Needing Response (or a note if none found)

- [ ] **PASS:** /daily shows my actual calendar and email

**Troubleshooting:**
- If calendar is empty: Check Step 2 passed
- If email section fails: The query might need adjustment for your inbox
- If command not found: Verify `daily.md` is in `.claude/commands/`

---

## Step 4: Add Your First Venture (Optional)

"Ventures" are projects, clients, or areas of focus you want Claude to know about.

### 4.1 Create a Venture Doc

Copy the template:

```bash
cp ventures/_TEMPLATE.md ventures/my-project.md
```

### 4.2 Fill It In

Open `ventures/my-project.md` and customize:

```markdown
# My Project Quick Glance

## What It Is
A mobile app for tracking daily habits.

## Current Status
Active - building MVP

## Key Links
- Repo: github.com/me/habit-tracker
- Slack: #habit-tracker
```

### Checkpoint

Ask Claude: *"What ventures do I have?"*

Claude should list your venture and its status.

- [ ] **PASS:** Claude knows about my venture

---

## You're Ready!

Congratulations! Your Productivity HQ is now operational.

### What You Have

- [x] IDE with Claude Code extension installed
- [x] HQ workspace configured
- [x] CLAUDE.md with your context
- [x] Google Workspace connected via MCP
- [x] `/daily` command working

### Daily Workflow

Start each day by running:

```
/daily
```

This gives you a prioritized view of your calendar, email, and top actions.

### Next Steps

**Customize further:**
- Add more ventures as you work on different projects
- Create custom commands for workflows you repeat often
- Add more MCP tools (Slack, GitHub, Linear, etc.)

**Learn more:**
- Join the community: [Claude Code for Productivity](https://theoperationsguide.com/community)
- Weekly office hours for live help
- Template library for more commands and skills

---

## Quick Reference

### Essential Commands

```bash
# Start Claude Code
claude

# Run daily briefing
/daily

# Quick briefing (just next 2 hours)
/daily quick
```

### File Locations

| File | Purpose |
|------|---------|
| `CLAUDE.md` | Workspace memory - Claude reads this every session |
| `.claude/commands/daily.md` | The /daily command logic |
| `.claude/settings.json` | Permissions |
| `~/.claude/settings.local.json` | MCP server configuration |
| `projects/` | Active project work - create folders here |
| `ventures/*.md` | Context about your ventures/businesses |
| `_archive/` | Completed projects (move here when done) |

---

## If Something Goes Wrong

### "localhost refused to connect"

**What it looks like:** Browser shows "This site can't be reached" after logging into Google.

**Why it happens:** Google tried to send you back to Claude, but the authentication server wasn't listening.

**How to fix:**
1. Make sure your terminal is still open with the auth command running
2. Go to Google Cloud Console → Credentials → Your OAuth client
3. Check that `http://localhost:8000/oauth2callback` is in the redirect URIs
4. Click Save and wait 2-3 minutes for changes to propagate
5. Run the auth command again

### "redirect_uri_mismatch"

**What it looks like:** Google shows an error page mentioning "redirect_uri_mismatch."

**Why it happens:** The URL in Google Console doesn't exactly match what the MCP server expects.

**How to fix:**
1. Look at the error message - it often shows the expected URL
2. Copy that exact URL
3. Add it to your Google Console redirect URIs (Credentials → Your OAuth client)
4. Save and wait 2-3 minutes
5. Try again

### "Access blocked: This app's request is invalid"

**What it looks like:** Google shows a scary error about invalid requests.

**Why it happens:** You haven't added yourself as a test user.

**How to fix:**
1. Go to Google Cloud Console → OAuth consent screen
2. Scroll down to "Test users"
3. Click **Add Users**
4. Add your Google email address
5. Save and try again

### "This app isn't verified" warning

**What it looks like:** Google shows a warning screen about unverified apps.

**Why it happens:** Your app is in "Testing" mode (which is fine for personal use).

**How to fix:**
1. This is expected! Click **Continue**
2. Review the permissions
3. Click **Allow**

### Claude says "MCP server not found" or similar

**What it looks like:** Claude doesn't recognize Google commands like "What's on my calendar?"

**Why it happens:** The MCP server isn't configured in Claude's settings.

**How to fix:**
1. Check that `~/.claude/settings.local.json` exists
2. Make sure it has a `mcpServers` section with your Google config
3. Verify the paths in the config point to your actual credential files
4. Restart Claude Code (close and reopen your IDE)

### Common Issues Quick Reference

| Problem | Solution |
|---------|----------|
| "Command not found" | Check file is in `.claude/commands/` |
| "MCP authentication failed" | Re-run OAuth flow for that tool |
| "Empty results" | Verify email in CLAUDE.md matches your account |
| "Claude doesn't remember" | Make sure CLAUDE.md is in workspace root |
| "localhost refused to connect" | Add redirect URI + keep terminal open during auth |

---

**Questions?** Ask in the community or email support@theoperationsguide.com
