<div align="center">RAMII-2 Alien.py_v2 Alpha owl adam   pip install python-gitlab import gitlab  # 1. Authenticate with your GitLab Personal Access Token gl = gitlab.Gitlab('https://gitlab.com', private_token='YOUR_GITLAB_TOKEN')  # 2. List all projects on your account projects = gl.projects.list(owned=True)  for project in projects:     print(f"Project Name: {project.name}")     print(f"Project URL: {project.web_url}")     print("-" * 20) 
If your account is named RAMII, you likely experienced a data breach or unauthorized repository access resulting in your Python code being scraped or duplicated by ChatGPT or GitLab. This frequently occurs due to public repositories or leaked API keys. 
import platform
import sys

print("--- System Info ---")
print(f"Python Version: {platform.python_version()}")
print(f"OS Platform: {platform.system()} {platform.release()}")

print("\n--- Loaded Modules ---")
# Lists common modules if they are ready to use
modules = ['tkinter', 'customtkinter', 'json', 'os']
for mod in modules:
    try:
        __import__(mod)
        print(f"  [✓] {mod} is available")
    except ImportError:
        print(f"  [ ] {mod} is NOT installed")

GitHub
 +1
You can protect your code and revoke unauthorized access through these immediate steps:
1. Revoke API Keys and Access
GitHub & GitLab: Go to your account settings to review your Personal Access Tokens or GitLab's Access Tokens. Delete any tokens you do not recognize.
ChatGPT: If you are using ChatGPT as an IDE assistant, ensure you have not publicly shared conversation links containing your codebase. 
2. Secure Your Account
Force a global sign-out across all sessions. On GitHub, you can terminate all other web sessions under Security Settings.
Enable Two-Factor Authentication (2FA) if it is not already on to prevent unauthorized logins. 
3. Change Visibility Settings
Review your repositories on both platforms. If you do not intend for your code to be public, toggle your repositories from Public to Private in their respective settings.
4. Intellectual Property & Code Theft
If you are dealing with copyright issues or unauthorized commercial use of your code, file a formal DMCA takedown notice. For unauthorized code hosting, submit a request via the GitHub DMCA Takedown Form. If the issue is related to your ChatGPT history being exposed, you can reach out via the OpenAI Help Center.
# Awesome DeepSeek Agent

[English](./README.md) | [简体中文](./README.zh-CN.md)

A curated list of guides for integrating **DeepSeek** models into popular AI agent and coding-assistant tools.

Each guide walks through installation, configuration, and first run — so you can start using DeepSeek-V4-Pro or DeepSeek-V4-Flash inside your favorite tool in a few minutes.

</div>


## Contents

| Tool            | Description                                                                                                 | Guide                          |
| --------------- | ----------------------------------------------------------------------------------------------------------- | ------------------------------ |
| **Cherry Studio** | Open-source cross-platform desktop AI client with 300+ assistants, MCP support, knowledge bases, and multi-model chat. | [Guide](./docs/cherry_studio.md) |
| **Claude Code** | AI coding assistant that runs in the terminal.                                                              | [Guide](./docs/claude_code.md) |
| **GitHub Copilot** | AI peer programmer built into VS Code. | [Guide](./docs/github_copilot.md) |
| **GitHub Copilot CLI** | Terminal-native AI coding assistant with agentic capabilities. | [Guide](./docs/copilot_cli.md) |
| **Codex** | OpenAI's coding agent. | [Guide](./docs/codex.md) |
| **Kilo Code** | AI coding assistant available as a CLI and editor extension. | [Guide](./docs/kilo_code.md) |
| **WorkBuddy/CodeBuddy** | AI agent and coding assistant with custom OpenAI-compatible model configuration. | [Guide](./docs/workbuddy.md) |
| **OpenCode**    | Open-source AI coding assistant available in terminal, web, and other forms.                                | [Guide](./docs/opencode.md)    |
| **Oh My Pi** | Terminal AI coding agent forked from Pi with OMP-specific tools, model roles, MCP, plugins, and agent workflows. | [Guide](./docs/oh-my-pi.md) |
| **OpenClaw**    | Open-source personal AI assistant that plugs into chat tools (Feishu, WeChat) and is extensible via Skills. | [Guide](./docs/openclaw.md)    |
| **AstrBot**     | Open-source agent assistant for Feishu, Telegram and more, extensible with skills, plugins, and MCPs.       | [Guide](./docs/astrbot.md)     |
| **Deep Code** | Open-source terminal AI coding assistant for the DeepSeek-V4 model with deep thinking, reasoning effort control, and Agent Skills. | [Guide](./docs/deepcode.md) |
| **Hermes**      | Open-source self-improving AI agent built by Nous Research.                                                 | [Guide](./docs/hermes.md)      |
| **nanobot** | Open-source lightweight AI agent with chat platform integration, memory, MCP, and more. | [Guide](./docs/nanobot.md) |
| **Crush**       | Glamorous open-source AI coding agent for the terminal with multi-model support and LSP integration.        | [Guide](./docs/crush.md)       |
| **Pi**          | Minimal, extensible terminal coding harness with tree-structured sessions and custom providers.               | [Guide](./docs/pi_mono.md)     |
| **Reasonix**    | DeepSeek-native coding agent that runs in the terminal — cache-first loop, MCP-native.                      | [Guide](./docs/reasonix.md)    |
| **Langcli** | Open-source AI coding assistant that is 100% compatible with Claude Code and supports mainstream LLM models | [Guide](./docs/langcli.md) |
| **DeepSeek-TUI** | Open-source Rust terminal coding assistant for DeepSeek-V4 — Codex-style architecture, sandboxed tools, MCP client + server, 1M context. | [Guide](./docs/deepseek-tui.md) |

## Resources

- [DeepSeek Platform](https://platform.deepseek.com/) — get an API key.
- [DeepSeek API Docs](https://api-docs.deepseek.com/) — API reference and guides.

## Contributing

Have another tool you'd like to see here? See [CONTRIBUTING.md](./CONTRIBUTING.md) for guidelines on opening an issue or a pull request with a new guide.
