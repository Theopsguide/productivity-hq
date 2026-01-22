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

**Time:** 5 minutes

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

## Step 2: Connect Google Workspace

**Time:** 20-30 minutes

This connects Claude to your Gmail and Google Calendar using MCP (Model Context Protocol). You'll create your own Google Cloud project with OAuth credentials - this is a one-time setup.

### Before You Start

**What you'll need:**
- A Google account (personal Gmail or Google Workspace)
- About 20-30 minutes for initial setup
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
3. In the popup, click **New Project** (top right)
4. Enter project details:
   - **Project name:** `Claude MCP` (or any name you like)
   - **Organization:** Leave as default (or "No organization")
5. Click **Create**
6. Wait a few seconds for the project to be created
7. Click **Select Project** when prompted, or select it from the dropdown

**Checkpoint:** You should see "Claude MCP" (or your project name) in the top dropdown.

---

### 2.2 Enable the APIs

Your project needs permission to access Gmail, Calendar, etc.

1. In the left sidebar, click **APIs & Services**
2. Click **Library** (or go to [API Library](https://console.cloud.google.com/apis/library))
3. Enable each of these APIs:

**Gmail API:**
1. Search for "Gmail API"
2. Click on **Gmail API** in the results
3. Click the blue **Enable** button
4. Wait for it to enable (you'll see a dashboard)

**Google Calendar API:**
1. Go back to the Library (click "Library" in left sidebar)
2. Search for "Google Calendar API"
3. Click on **Google Calendar API**
4. Click **Enable**

**Google Drive API (optional):**
1. Search for "Google Drive API"
2. Click **Enable** (gives Claude access to your Drive files)

**Checkpoint:** Go to **APIs & Services** → **Enabled APIs** and verify you see Gmail and Calendar listed.

---

### 2.3 Configure OAuth Consent Screen

This tells Google what your app is and who can use it.

1. In the left sidebar, click **APIs & Services** → **OAuth consent screen**
2. Select **User Type:**
   - **External** - Choose this (works for any Google account)
   - **Internal** - Only shows if you have Google Workspace (limits to your org)
3. Click **Create**

**Fill in App Information:**

| Field | What to Enter |
|-------|---------------|
| **App name** | `Claude MCP` |
| **User support email** | Your email address |
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

**Checkpoint:** OAuth consent screen should show "Testing" status with your app name.

---

### 2.4 Create OAuth Credentials

Now create the credentials file that the MCP server will use.

1. In the left sidebar, click **APIs & Services** → **Credentials**
2. Click **+ Create Credentials** (top of page)
3. Select **OAuth client ID**
4. For **Application type**, select **Desktop app**
5. **Name:** `Claude MCP Client`
6. Click **Create**

**Download Your Credentials:**

1. A popup appears with your Client ID and Secret
2. Click **Download JSON**
3. Save the file as `credentials.json`
4. **Store this file securely** - you'll need it for the MCP server

**Checkpoint:** You should have a `credentials.json` file downloaded (about 1KB).

---

### 2.5 Install the MCP Server

Now install the MCP server that will use your credentials.

**Recommended:** [google_workspace_mcp](https://github.com/taylorwilsdon/google_workspace_mcp) - Most feature-complete

**Option A: Smithery CLI (Easiest)**
```bash
npx -y @smithery/cli install mcp-gsuite --client claude
```
Then follow prompts to add your `credentials.json`.

**Option B: Manual Setup**
```bash
# Clone the repo
git clone https://github.com/taylorwilsdon/google_workspace_mcp.git
cd google_workspace_mcp

# Install dependencies
npm install

# Copy your credentials.json to the project folder
cp ~/Downloads/credentials.json .

# Run authentication
npm run auth
```

See the [MCP Servers Directory](https://github.com/modelcontextprotocol/servers) for other options.

---

### 2.6 Authenticate

1. Run the MCP server's authentication command
2. A browser window will open automatically
3. Sign in with the **same Google account** you added as a test user
4. You'll see a warning: "Google hasn't verified this app"
   - Click **Continue** (this is expected for your own app)
5. Review the permissions and click **Allow**
6. Close the browser - authentication is complete

**Checkpoint:** You should see a success message in your terminal.

---

### 2.7 Update CLAUDE.md

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

## Step 3: Run Your First Briefing

**Time:** 5 minutes

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

**Time:** 5 minutes

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
| `projects/` | Active project work - create folders here |
| `ventures/*.md` | Context about your ventures/businesses |
| `_archive/` | Completed projects (move here when done) |

### Common Issues

| Problem | Solution |
|---------|----------|
| "Command not found" | Check file is in `.claude/commands/` |
| "MCP authentication failed" | Re-run OAuth flow for that tool |
| "Empty results" | Verify email in CLAUDE.md matches your account |
| "Claude doesn't remember" | Make sure CLAUDE.md is in workspace root |

---

**Questions?** Ask in the community or email support@theoperationsguide.com
