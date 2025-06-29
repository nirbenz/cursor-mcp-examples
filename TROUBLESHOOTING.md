# MCP Troubleshooting Guide

This guide helps resolve common issues when setting up and using the Cursor + MCP workflow.

## GitHub MCP Issues

### Token Scopes
**Problem**: "No result from tool" or permission errors
**Fix**: Check your GitHub token has the right scopes:
- ✅ `repo` (Full control of private repositories)
- ✅ `read:org` (Read org and team membership)  
- ✅ `user:email` (Access user email addresses)

### Branch Issues
**Problem**: "Invalid base branch" errors
**Solution**: Ensure base branch exists on remote
```bash
# Push your main branch to remote
git push -u origin main
```

### Repository Structure
**Problem**: MCP can't access private repositories
**Solution**: Private repos need special handling - ensure token has `repo` scope

### Token Testing
Test your GitHub token:
```bash
# Test GitHub token scopes
curl -I -H "Authorization: token YOUR_TOKEN" https://api.github.com/user

# Check repository access  
curl -H "Authorization: token YOUR_TOKEN" https://api.github.com/repos/OWNER/REPO
```

## Linear MCP Issues

### Authentication Problems
**Problem**: Linear authentication fails or shows wrong account
**Solution**: Clear auth cache and re-authenticate
```bash
# Clear Linear auth cache
rm -rf ~/.mcp-auth
# Restart Cursor and try again
```

### Team IDs and User IDs
**Problem**: Can't find team/user IDs for rule configuration
**Solution**: Get IDs via Linear MCP:
```bash
# In Cursor chat:
> List my Linear teams and users
```

### Multi-Account Issues
**Problem**: Switching between Linear accounts doesn't work
**Solution**: Clear auth cache between account switches
```bash
rm -rf ~/.mcp-auth
```

### Label Management
**Problem**: Labels not showing up or applying incorrectly
**Solution**: 
- Use descriptive names in rules
- Store actual Linear label IDs, not names
- Verify labels exist in your Linear workspace

## Context7 MCP Issues

### Rate Limits
**Problem**: Context7 requests failing or slow
**Solution**: Be aware of API quotas and usage patterns

### Version Selection
**Problem**: Getting outdated dependency information
**Solution**: Always check for latest versions, Context7 may cache data

### Integration Issues
**Problem**: Dependencies not updating properly
**Solution**: Context7 works best with existing dependency management tools

## General MCP Issues

### Docker Issues (GitHub MCP)
**Problem**: GitHub MCP not starting
**Solution**: Ensure Docker is running
```bash
# Check Docker status
docker ps
# Start Docker if needed
```

### Node.js Issues (Linear MCP)
**Problem**: Linear MCP not starting
**Solution**: Ensure Node.js is installed
```bash
# Check Node.js version
node --version
# Should be v16+ for MCP compatibility
```

### Configuration Issues
**Problem**: MCP servers not loading
**Solution**: Verify `~/.cursor/mcp.json` configuration:
```bash
# Check configuration file exists and has correct format
cat ~/.cursor/mcp.json
```

## Common Error Messages

### "No result from tool"
**Causes**:
- Missing or invalid tokens
- Incorrect branch structure
- Permission issues
- Server not running (Docker/Node.js)

**Solutions**:
1. Verify token permissions
2. Check branch exists on remote
3. Restart Cursor
4. Check Docker/Node.js status

### "Validation Failed"
**Causes**:
- Incorrect branch names
- Repository structure issues
- Missing required fields

**Solutions**:
1. Check branch naming conventions
2. Verify repository structure
3. Ensure all required fields are provided

### "Authentication Required"
**Causes**:
- Expired tokens
- Cleared auth cache
- Multi-account confusion

**Solutions**:
1. Regenerate tokens
2. Re-authenticate via MCP prompts
3. Clear auth cache for fresh start

## Issues We've Solved

### "Invalid base branch"
**Problem**: Remote base branch doesn't exist
**Solution**: Push main branch first: `git push -u origin main`

### "No result from tool"  
**Problem**: Usually permission or branch structure issues
**Solution**: Check token scopes and branch structure

### "Validation Failed"
**Problem**: Branch names or repository structure incorrect
**Solution**: Verify branch naming and repository settings

## Getting Help

If you're still experiencing issues:

1. **Check Prerequisites**: Ensure all prerequisites from SETUP_GUIDE.md are met
2. **Review Configuration**: Compare your setup with the working examples
3. **Test Components**: Test each MCP server individually
4. **Check Logs**: Look for error messages in Cursor's output
5. **Fresh Start**: Sometimes clearing all auth and starting over helps

## Pro Tips

- **Test incrementally**: Set up one MCP server at a time
- **Use descriptive token names**: Makes tracking and debugging easier
- **Set token expiration**: Security best practice that forces periodic updates
- **Keep backups**: Save working configurations before making changes
- **Document customizations**: Note any project-specific changes you make 