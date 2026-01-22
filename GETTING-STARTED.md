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

This connects Claude to your Gmail and Google Calendar using MCP (Model Context Protocol).

### 2.1 Create Google Cloud Project & OAuth Credentials

First, you need to set up OAuth credentials in Google Cloud Console.

**Go to:** [Google Cloud Console](https://console.cloud.google.com/)

#### Step 1: Create a New Project

1. Click the project dropdown (top left, next to "Google Cloud")
2. Click **New Project**
3. Name it something like "Claude MCP" or "Productivity HQ"
4. Click **Create**
5. Make sure the new project is selected

#### Step 2: Enable APIs

1. Go to **APIs & Services** → **Library** (left sidebar)
2. Search for and enable these APIs:
   - **Gmail API** - Click it, then click **Enable**
   - **Google Calendar API** - Click it, then click **Enable**
   - **Google Drive API** (optional) - For file access

#### Step 3: Configure OAuth Consent Screen

1. Go to **APIs & Services** → **OAuth consent screen**
2. Select **External** (unless you have Google Workspace admin access)
3. Click **Create**
4. Fill in required fields:
   - **App name:** "Claude MCP" (or anything you like)
   - **User support email:** Your email
   - **Developer contact:** Your email
5. Click **Save and Continue**
6. On **Scopes** page, click **Add or Remove Scopes**
7. Add these scopes (search or paste):
   - `https://www.googleapis.com/auth/gmail.modify`
   - `https://www.googleapis.com/auth/calendar`
   - `https://www.googleapis.com/auth/drive.readonly` (optional)
8. Click **Update**, then **Save and Continue**
9. On **Test users** page, click **Add Users**
10. Add your Google email address
11. Click **Save and Continue**

#### Step 4: Create OAuth Credentials

1. Go to **APIs & Services** → **Credentials**
2. Click **Create Credentials** → **OAuth client ID**
3. Application type: **Desktop app**
4. Name: "Claude MCP Client"
5. Click **Create**
6. Click **Download JSON** (save as `credentials.json`)

**Keep this file safe** - you'll need it for the MCP server setup.

---

### 2.2 Install the MCP Server

**Recommended:** [google_workspace_mcp](https://github.com/taylorwilsdon/google_workspace_mcp)

**Option A: Smithery CLI**
```bash
npx -y @smithery/cli install mcp-gsuite --client claude
```

**Option B: Manual Setup**
```bash
# Clone the repo
git clone https://github.com/taylorwilsdon/google_workspace_mcp.git
cd google_workspace_mcp

# Install dependencies
npm install

# Copy your credentials.json to the project folder
cp ~/Downloads/credentials.json .
```

See the [MCP Servers Directory](https://github.com/modelcontextprotocol/servers) for other options.

---

### 2.3 Authenticate

1. Run the MCP server's authentication command
2. A browser window will open
3. Sign in with the Google account you added as a test user
4. Click **Continue** (even if it shows "unverified app" warning)
5. Grant all requested permissions
6. The credentials will be saved locally

### 2.3 Update CLAUDE.md

Uncomment and fill in the Google Workspace section:

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
