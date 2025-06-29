# Examples

Real examples from this project showing how Cursor + MCP integration actually works.

## What's here

### Documentation
- `workflow_examples.md` - 6 detailed examples including the main PR workflow
- `README.md` - This file

### Setup stuff (in `../setup/`)
- `mcp.json.example` - MCP server config template
- `SETUP_GUIDE.md` - How to set it up
- `cursor/` - Cursor directory template for your repo

## What this shows

### The difference
**Before**: Spending 8-10 minutes on each PR, forgetting to link issues half the time
**After**: 30 seconds, everything linked properly every time

### Real Results from This Repository
- ✅ **47 files changed** in major package restructure
- ✅ **Perfect semantic commit** generated from code analysis  
- ✅ **Linear issue CML-14** created automatically
- ✅ **GitHub PR #1** with comprehensive description
- ✅ **Full integration** between Linear and GitHub
- ✅ **Zero manual intervention** required

## How to use these examples

### For learning
Read through to understand:
- How the MCP calls actually work
- What kind of output you get
- How the tools talk to each other
- Real problem solving, not demo stuff

### For copying this setup
Use the setup files to get the same workflow:
```bash
cp setup/mcp.json.example .cursor/mcp.json
cp -r setup/cursor_rules .cursor/rules
# Then follow setup/SETUP_GUIDE.md
```

## What's next

After you set this up, you can extend it:
- Add more MCPs (Slack, Jira, Notion, etc.)
- Custom rules for your workflows
- Team-specific configurations
- Advanced automation (deployment triggers, etc.)

## Getting help

If you're setting this up:
1. Start with `SETUP_GUIDE.md`
2. Use the exact configs from `setup/`
3. Look at the real examples here
4. Test with small changes first

This isn't a demo - it's how I actually work now and it saves a ton of time. 