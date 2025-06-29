# Setup Guide: Cursor + MCP Workflow

This guide helps you replicate the exact workflow demonstrated in the presentation.

## Prerequisites

- [Cursor IDE](https://cursor.sh/) installed
- GitHub account with personal access token
- Linear account (optional, but recommended)
- Docker installed (for GitHub MCP)
- Node.js installed (for Linear MCP)

## Step 1: Configure MCP Servers

### 1.1 Copy MCP Configuration
```bash
cp setup/mcp.json.example ./cursor/mcp.json
```

### 1.2 Generate GitHub Token
1. Go to [GitHub Personal Access Tokens](https://github.com/settings/tokens)
2. Click "Generate new token (classic)"
3. Select these scopes:
   - ✅ `repo` (Full control of private repositories)
   - ✅ `read:org` (Read org and team membership)
   - ✅ `user:email` (Access user email addresses)
4. Copy the token and replace `ghp_YOUR_GITHUB_TOKEN_HERE` in `.cursor/mcp.json`

**Security notes:**
- Don't commit tokens to your repo
- Set expiration dates if you can
- Use descriptive names to track token usage
- Delete old tokens when you're done with them

### 1.3 Linear Authentication
Linear MCP uses OAuth - it'll prompt you to authenticate when you first use it.

**Important stuff:**
- Linear uses internal IDs for teams, users, and labels
- You need to store these IDs (once) in your rule files
- You can get these IDs when you first call Linear MCP
- **Multi-account users**: Go to `~/.mcp-auth` and remove old authentications if switching accounts

## Step 2: Setup Cursor Rules

### 2.1 Create Rules Directory
```bash
mkdir -p ./cursor/rules
```

### 2.2 Copy Rule Files
```bash
cp setup/cursor_rules/* ./cursor/rules/
```

### 2.3 Update Rule Configuration
Edit the rule files to match your setup:

**In `git_rules.mdc`:**
- Replace `yourusername` with your GitHub username
- Replace `cursor-mcp-examples` with your repository name

**In `linear_rules.mdc`:**
- Update team IDs to match your Linear workspace
- Update user IDs to match your Linear account

### 2.4 How Cursor Rules Work
Rules control when and how Cursor applies different behaviors:

| Rule Type        | Usage                                            | `description` Field | `globs` Field         | `alwaysApply` field |
| ---------------- | ------------------------------------------------ | ------------------- | --------------------- | ------------------- |
| Agent Selected   | Agent sees description and chooses when to apply | critical            | blank                 | false               |
| Always           | Applied to every chat and cmd-k request          | blank               | blank                 | true                |
| Auto Select      | Applied to matching existing files               | blank               | critical glob pattern | false               |
| Auto Select+desc | Better for new files                             | included            | critical glob pattern | false               |
| Manual           | User must reference in chat                      | blank               | blank                 | false               |

## Step 3: Test the Setup

### 3.1 Test GitHub MCP
1. Open any repository in Cursor
2. In chat, type: "List my recent repositories"
3. You should see your GitHub repos listed

### 3.2 Test Linear MCP
1. In chat, type: "List my Linear issues"
2. You should see your Linear issues (after OAuth prompt)

### 3.3 Test the Full Workflow
1. Make some changes to a file
2. Stage the changes: `git add .`
3. In chat, type: `@git_rules do my PR routine`
4. Watch the magic happen!

## Step 4: Customize for Your Workflow

### 4.1 Update Team Information
Get your Linear team and user IDs:
```bash
# In Cursor chat:
> List my Linear teams and users
```

Update `.cursor/rules/linear_rules.mdc` with the actual IDs.

### 4.2 Customize Git Rules
Edit `.cursor/rules/git_rules.mdc` to match your:
- Repository naming conventions
- Commit message preferences  
- PR review process
- Branch naming strategy

### 4.3 Add Project-Specific Rules
Create additional rule files for specific projects:
```bash
# Example: Python-specific rules
touch ./cursor/rules/python_rules.mdc
```

## Troubleshooting

### Common Issues

**"No result from tool" errors:**
- Check *global* MCP server configuration in `~/.cursor/mcp.json`
- Verify tokens have correct permissions
- Make sure Docker is running (for GitHub MCP)

**"Invalid base branch" errors:**
- Make sure your main branch exists on remote
- Push your main branch: `git push -u origin main`

**Linear authentication issues:**
- Clear auth cache: `rm -rf ~/.mcp-auth`
- Restart Cursor and try again

### Token Security

**Best practices:**
- Use environment variables instead of hardcoding tokens
- Set token expiration dates
- Use descriptive token names for tracking
- Revoke old tokens when no longer needed

**Alternative storage:**
```bash
# Use environment variables
export GITHUB_PERSONAL_ACCESS_TOKEN="ghp_your_token"
# Then reference in mcp.json as: "${GITHUB_PERSONAL_ACCESS_TOKEN}"
```

## What You'll Get

Once set up, you'll have:

- Automatic PR creation with good descriptions  
- Linear issue integration with status sync  
- Semantic commit messages generated from code analysis  
- No context switching between tools  
- Consistent workflow across all projects  

## Example Commands

```bash
# Full PR workflow
> @git_rules do my PR routine

# Just create commit message
> Generate a semantic commit message for these changes

# Create Linear issue
> Create a Linear issue for this feature

# Link existing issue
> Link this PR to Linear issue CML-123
```

## Support

If you encounter issues:
1. Check the [troubleshooting section](#troubleshooting)
2. Review the actual working example in this repository
3. Compare your setup with the `examples/` directory
4. Ensure all prerequisites are installed correctly

---

**Result:** You'll have the same 30-second PR workflow demonstrated in the presentation! 