# Workflow Examples

Actual commands and outputs from this project showing how the Cursor + MCP setup works in practice.

## Example 1: The Main Workflow - Complete PR Routine

This is the workflow I demo in presentations.

### Overview
This repo contains the actual workflow from the presentation.

**What happened:**
- **Linear Issue**: [CML-14](https://linear.app/yourteam/issue/CML-14/restructure-telegram-linker-to-modern-package-architecture)
- **GitHub PR**: [#1](https://github.com/yourusername/cursor-mcp-examples/pull/1) 
- **Time**: 30 seconds vs the usual 10+ minutes
- **Files**: 47 files changed in a major refactor

### The comparison

#### Before (doing it manually):
```bash
# Takes 8-10 minutes and usually sucks
git add .
git commit -m "stuff"  # ← Awful message
git push
# Open browser, go to GitHub
# Fill out PR form
# Try to remember to link Linear issue
# Copy-paste descriptions around
# Set reviewers and labels
```

#### After (with Cursor + MCP):
```bash
# Takes 30 seconds
> @git_rules do my PR routine
```

**Command:**
```
@git_rules do my PR routine
```

**What happened:**
1. Found 47 staged files
2. Analyzed the package restructure 
3. Generated a proper commit message
4. Created Linear issue CML-14 automatically
5. Created GitHub PR #1 with a good description
6. Linked the Linear issue in PR comments
7. Set reviewers and labels

**Results:**
- **Commit**: `refactor: restructure to telegram-linker package with modern architecture`
- **Linear Issue**: [CML-14](https://linear.app/yourteam/issue/CML-14)
- **GitHub PR**: [#1](https://github.com/yourusername/cursor-mcp-examples/pull/1)
- **Time**: 30 seconds total

### What actually got generated

#### Commit Message:
```
refactor: restructure to telegram-linker package with modern architecture

- Migrate from flat src/ to organized telegram_linker/ package structure
- Update dependencies: numpy 2.2.6, openai 1.76.0, langchain-* latest
- Add ruff linting and structured LLM types
- Reorganize configs: remove streamlit, add main_app_cfg
- Implement proper separation: common/, lib/, llms/, prompts/
- Add cache handler and link mapper components
- Update model configs for latest OpenAI models (o3, o4-mini)
```

#### PR Description:
```markdown
Major refactoring implementing modern Python package structure with improved separation of concerns.

## Key Changes
- **Package Structure**: Migrated from flat src/ to organized telegram_linker/ package
- **Dependencies**: Updated to latest versions (numpy 2.2.6, openai 1.76.0, langchain ecosystem)  
- **Architecture**: Proper separation with common/, lib/, llms/, prompts/ modules  
- **Tooling**: Added ruff linting and structured LLM types
- **Configuration**: Reorganized configs, removed streamlit components
- **Components**: Added cache handler, link mapper, enhanced telegram bot

## Technical Improvements
- Modern Python >=3.12 requirement
- Proper package structure for maintainability
- Updated model configs for latest OpenAI models
- Enhanced LLM integration with structured types
- Better error handling and logging
```

#### Linear Issue Created:
- **Title**: "Restructure telegram-linker to modern package architecture"
- **Status**: Set to "In Review" 
- **Assignee**: Auto-assigned
- **Labels**: "Feature" label
- **Description**: Technical breakdown

### Why this is useful:
- No manual work
- Consistent every time
- Never forget to link things
- Professional quality output

This is the actual workflow I used to build this presentation.

---

## Example 2: Linear Issue Analysis

**Command:**
```
Analyze Linear issue CML-17 and suggest implementation approach
```

**What happened:**
1. Fetched issue details via Linear MCP
2. Read the current `page_analysis_prompts.py` file
3. Analyzed existing prompt structure
4. Generated implementation suggestions

**Output:**
> **CML-17: Optimize LLM prompt templates**
> 
> Current issue shows prompt optimization needs for better link categorization. Based on the code analysis:
> 
> **Current Problems:**
> - Verbose prompts causing slow inference (500ms+ per request)
> - Generic categorization (60% of links marked as "general")
> - Inconsistent results for similar content
> 
> **Recommended Approach:**
> 1. Implement few-shot prompting with 3-5 examples per category
> 2. Add confidence scoring to prompt output
> 3. Test with O3 model configuration for better reasoning
> 4. Create A/B testing framework for prompt variants
> 
> **Expected Impact:**
> - 30% faster response times
> - 20% better categorization accuracy
> - More consistent results across content types

---

## Example 3: Dependency Management with Context7

**Command:**
```
Add newspaper3k library for link preview extraction using context7
```

**What happened:**
1. Used Context7 MCP to get latest newspaper3k information
2. Updated pyproject.toml with proper version
3. Generated implementation examples
4. Suggested best practices

**Output:**
> Added newspaper3k v0.2.8 to dependencies. Here's the recommended implementation:
> 
> ```python
> from newspaper import Article
> 
> async def extract_link_preview(url: str) -> LinkPreview:
>     article = Article(url)
>     article.download()
>     article.parse()
>     
>     return LinkPreview(
>         title=article.title,
>         description=article.meta_description,
>         image_url=article.top_image,
>         author=article.authors[0] if article.authors else None
>     )
> ```
> 
> **Integration Points:**
> - Add to `link_mapper.py` for rich metadata extraction
> - Cache results in `cache_handler.py` to avoid repeated requests
> - Handle failures gracefully with fallback to URL-only storage

---

## Example 4: Bug Investigation

**Command:**
```
Investigate the cache memory leak issue from Linear CML-18
```

**What happened:**
1. Fetched Linear issue CML-18 details
2. Analyzed `cache_handler.py` code
3. Identified potential memory leak patterns
4. Suggested fixes with code examples

**Output:**
> **CML-18: Cache Memory Leak Analysis**
> 
> Found the issue in `cache_handler.py` line 45-67:
> 
> **Problem:**
> ```python
> # Current problematic code
> def cleanup_expired(self):
>     for key, (value, timestamp) in self._cache.items():  # ← Memory leak here
>         if time.time() - timestamp > self.ttl:
>             del self._cache[key]  # ← Modifying dict during iteration
> ```
> 
> **Fix:**
> ```python
> def cleanup_expired(self):
>     expired_keys = [
>         key for key, (value, timestamp) in self._cache.items()
>         if time.time() - timestamp > self.ttl
>     ]
>     for key in expired_keys:
>         self._cache.pop(key, None)
> ```
> 
> **Additional Improvements:**
> - Add memory monitoring with `psutil`
> - Implement proper LRU eviction using `collections.OrderedDict`
> - Add cache metrics for observability

---

## Example 5: Project Status Overview

**Command:**
```
Give me a status overview of all Linear issues for this project
```

**Response:**
> **CursorML Team - Project Status**
> 
> **🔴 Urgent (1)**
> - CML-18: Fix cache handler memory leak (In Progress)
> 
> **🟡 High Priority (2)**  
> - CML-16: Add link preview extraction (Backlog)
> - CML-17: Optimize LLM prompts (In Progress)
> 
> **🟢 Medium Priority (3)**
> - CML-15: Implement rate limiting (Todo)
> - CML-20: Migrate to O3 model (Done ✅)
> - CML-21: Add unit tests (Backlog)
> 
> **🔵 Current Sprint (2)**
> - CML-17: Prompt optimization (50% complete)
> - CML-18: Memory leak fix (investigation phase)
> 
> **Recent Completion:**
> - CML-14: Package restructure (Merged ✅)
> - CML-20: O3 model migration (Deployed ✅)

---

## Example 6: Quick Fixes

**Command:**
```
Create a hotfix for the urgent cache issue and deploy it
```

**What happened:**
1. Created hotfix branch from main
2. Applied memory leak fix
3. Generated focused commit message
4. Created expedited PR with proper urgency labels

**Generated Commands:**
```bash
git checkout main
git pull origin main
git checkout -b hotfix/cache-memory-leak-fix
# Applied the fix to cache_handler.py
git add src/telegram_linker/lib/cache_handler.py
git commit -m "hotfix: fix memory leak in cache cleanup during iteration

- Fix unsafe dict modification during iteration in cleanup_expired()
- Add proper expired key collection before deletion
- Prevents memory accumulation in long-running sessions
- Critical fix for production stability

Fixes: CML-18"
git push origin hotfix/cache-memory-leak-fix
```

**PR Created:**
- Title: `[HOTFIX] Fix critical cache memory leak in production`
- Labels: `hotfix`, `urgent`, `bug`
- Reviewers: Auto-assigned based on rules
- Linear: Linked to CML-18, status updated to "In Review"

---

## What this shows

These examples show how Cursor + MCP changes development:

| **Old way**             | **With Cursor + MCP**         |
| ----------------------- | ----------------------------- |
| 8-10 minutes per PR     | 30 seconds automated          |
| Manual commit messages  | AI-generated semantic commits |
| Switching between tools | Single interface              |
| Manual issue tracking   | Auto-linked and synced        |
| Inconsistent processes  | Perfect consistency           |

**Result**: Development speed increased by 10x with no quality loss. 