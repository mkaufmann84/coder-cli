
<START GUIDE FOR CODER>
# Code CLI - Headless Mode Guide

Run Code non-interactively for CI/CD pipelines, scripts, and automation.

## Quick Start

```bash
# Basic headless execution
code exec "update the README with installation instructions"

# With full automation (workspace-write sandbox + on-failure approval)
code exec --full-auto "run tests and fix any failures"

# With specific model
code exec -m claude-sonnet-4.5 "analyze security vulnerabilities"

# Resume previous session
code exec resume --last "continue the task"
code exec resume <SESSION_ID> "add more changes"
```

## Common Flags

| Flag | Description |
|------|-------------|
| `--full-auto` | Auto-execute with workspace sandbox (recommended for CI) |
| `-m, --model <MODEL>` | Specify model (e.g., `gpt-5`, `claude-sonnet-4.5`) |
| `-C, --cd <DIR>` | Set working directory |
| `--no-search` | Disable web search |
| `-c <KEY>=<VALUE>` | Override config (e.g., `-c approval_policy=never`) |

## Multi-Agent Commands

Leverage powerful orchestration with slash commands in headless mode:

```bash
# Create a comprehensive plan with multiple AI agents working in parallel
code exec "/plan implement OAuth authentication system"

# Solve complex problems using diverse model perspectives
code exec "/solve debug the memory leak in worker threads"

# Execute coding tasks with multiple models collaborating
code exec "/code refactor the API layer for better testability"
```

**How it works**: Commands like `/plan`, `/solve`, and `/code` spawn multiple AI agents (GPT-5, Claude Sonnet, Claude Opus, Gemini) that work in parallel. Each agent tackles the problem independently, then the orchestrator synthesizes the best solution from all approaches.

## CI/CD Examples

### GitHub Actions
```yaml
- name: Update changelog
  run: |
    export OPENAI_API_KEY="${{ secrets.OPENAI_KEY }}"
    code exec --full-auto "update CHANGELOG for release v1.2.0"

- name: Multi-agent architecture review
  run: |
    code exec "/plan migrate authentication to OAuth 2.0"
```

### Scripting
```bash
#!/bin/bash
# Fix all TODO comments in source files
code exec --full-auto "find all TODO comments and implement them"

# Resume if needed
if [ -f .last_session ]; then
  code exec resume $(cat .last_session) "complete remaining tasks"
fi
```

## Input Methods

```bash
# Direct argument
code exec "your task here"

# Via stdin
echo "refactor authentication module" | code exec

# With images
code exec --image screenshot.png "describe what's wrong with this UI"
```

## Session Management

```bash
# Start a task
code exec "implement user login" > session.txt

# Extract session ID from output
SESSION_ID=$(grep "resume" session.txt | awk '{print $NF}')

# Continue later
code exec resume $SESSION_ID "add password reset feature"
```

## Safety Modes

### Full Auto (Recommended for CI)
```bash
code exec --full-auto "task"
# Runs with: workspace-write sandbox + approval on-failure only
```

### Manual Control
```bash
code exec -a always "task"  # Always ask for approval
code exec -a never "task"   # Never ask (dangerous!)
code exec -s none "task"    # No sandbox (use in Docker/VM only)
```

### Extremely Dangerous (Externally Sandboxed Environments Only)
```bash
code exec --yolo "task"  # Skip ALL approvals and sandboxing
```

## Output & Logging

```bash
# Capture output
code exec "task" > output.txt 2>&1

# Enable verbose logging
RUST_LOG=info code exec "task"

# Debug mode (saves request/response JSON)
code exec --debug "task"
# Logs saved to ~/.code/debug_logs/
```

## Tips

1. **Always use `--full-auto` in CI** - It's the safest automation mode
2. **Set `OPENAI_API_KEY` or equivalent** - Required for headless operation
3. **Use absolute paths** - Avoid confusion with working directories
4. **Resume sessions** - Continue long-running tasks without losing context
5. **Check exit codes** - Non-zero indicates failure

## Environment Variables

```bash
export OPENAI_API_KEY="sk-..."        # OpenAI authentication
export ANTHROPIC_API_KEY="sk-ant-..."  # Anthropic authentication
export RUST_LOG="info"                # Logging level
export CODE_HOME="~/.code"            # Config directory
```

## Exit Codes

- `0` - Success
- Non-zero - Error (check output for details)

---

**Note**: Replace `code` with `codex` if using upstream OpenAI Codex CLI.
<END GUIDE FOR CODER/>