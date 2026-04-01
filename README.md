# Mattermost Playbook Generator Prompt

An LLM prompt that generates valid, importable Mattermost Playbook JSON files from a plain-English description of any process or workflow.

## Overview

[Mattermost Playbooks](https://mattermost.com/playbooks/) allow teams to codify repeatable processes — incident response, release management, customer onboarding, and so on — as structured checklists with built-in automation, status updates, and retrospectives.

Creating a playbook from scratch can be time-consuming, particularly if you're starting with an existing Standard Operating Procedure (SOP) document and need to translate it into Mattermost's JSON import format.

This repository contains a carefully crafted LLM prompt that does that translation for you. Feed it a description of your process (or upload an existing SOP document), and it will produce a JSON file that imports directly into Mattermost — no manual JSON editing required.

The prompt has been validated against the actual Mattermost Playbooks import parser using exports from a running instance, so the output conforms to the exact schema that Mattermost expects.

## Prerequisites

- Access to an LLM that supports long system prompts (Claude, ChatGPT, Gemini, etc.)
- A Mattermost instance with the Playbooks plugin enabled
- Sufficient permissions to import playbooks on your Mattermost instance

## Quick Start

### 1. Download the prompt

Download [`mattermost-playbook-generator-prompt.md`](mattermost-playbook-generator-prompt.md) from this repository, or clone the repo:

```bash
git clone https://github.com/jlandells/mattermost-playbook-generator.git
```

### 2. Upload the file and describe your use case

Open your preferred LLM tool, start a new conversation, and upload the Markdown file. In your message, reference the file and describe the process you want to turn into a playbook. For example:

> Using the attached Markdown file, generate a Mattermost Playbook for our customer onboarding process. The process typically takes 2 weeks and involves contract signing, environment provisioning, technical kickoff, integration setup, user training, and go-live sign-off.

The more detail you provide, the better the result. See the [prompting examples](#how-to-prompt-the-llm) below for more ideas.

### 3. Save and import the JSON

Save the LLM's JSON output to a `.json` file, then import it into Mattermost:

1. Navigate to **Playbooks** from the product menu
2. Click the **+** to the right of the team name
3. Select **Import Playbook**
4. Choose your `.json` file and select the target team

## How to Prompt the LLM

### Basic — describe a process from scratch

> Using the attached Markdown file, generate a Mattermost Playbook for handling customer-reported security vulnerabilities. We need triage, investigation, remediation, customer communication, and post-mortem stages. We use Jira for tracking and have Mattermost Calls enabled.

### From an existing SOP document

If you already have a Standard Operating Procedure documented as a PDF, Word document, or similar, you can upload it alongside the prompt file:

> Using the attached Markdown file, generate a Mattermost Playbook that follows our SOP in the attached PDF.

This works particularly well with LLMs that support multi-file uploads (such as Claude). The LLM will read your SOP and map its steps into the correct Playbook JSON structure.

### Refining the output

If the first result isn't quite right, you can iterate:

> That's good, but can you split the "Environment Setup" checklist into two separate checklists — one for infrastructure provisioning and one for application configuration? Also, add a metric for tracking setup duration.

## Saving the JSON Output

Not all LLMs present their output in the same way. Depending on which tool you're using:

- **Claude (claude.ai)** — Claude will typically present the JSON in a code block. Click the **Copy** button on the code block, then paste into a text editor and save as a `.json` file.
- **Claude with computer use / file creation** — Claude may create the file directly for you to download.
- **ChatGPT** — The JSON will appear in a code block. Use the **Copy code** button, paste into a plain text editor (such as Notepad, TextEdit, VS Code, or Vim), and save with a `.json` extension.
- **Other LLMs** — Copy the JSON from the response, paste into any plain text editor, and save as `.json`. Ensure you're using a plain text editor, not a word processor — saving from Microsoft Word or Google Docs will produce an invalid file.

**Important:** Whichever method you use, ensure the file is saved with a `.json` extension and contains only the JSON content — no surrounding commentary, no Markdown code fences, and no extra whitespace before or after the JSON.

## Post-Import Customisation

The generated playbook is deliberately free of instance-specific configuration (user IDs, channel IDs, team IDs, webhook URLs, etc.), since these differ between Mattermost installations. After importing, you will want to configure:

- **Members and invited groups** — Add the teams and individuals who should be involved in runs
- **Broadcast channels** — Set channels to receive status update notifications
- **Webhook integrations** — Add any webhook URLs for external tool notifications
- **Slash commands** — Adjust commands to match the integrations available on your instance
- **Channel naming template** — Set a naming pattern for run channels
- **Welcome message** — Add or refine the message shown when users join a run channel
- **Attributes and conditions** *(Mattermost v11.1+)* — Consider whether your playbook would benefit from custom attributes (e.g. severity, category, or linked ticket ID) and conditional tasks that adapt the workflow based on attribute values. These are configured through the Mattermost UI after import.

## Limitations

- The quality of the generated playbook depends on the quality of the use case description. Vague descriptions produce vague playbooks — be specific.
- LLMs can occasionally produce invalid JSON (missing commas, unclosed brackets, etc.). If the import fails, try pasting the JSON into a validator such as [jsonlint.com](https://jsonlint.com/) to identify syntax errors.

## Contributing

We welcome contributions from the community! Whether it's a bug report, a feature suggestion,
or a pull request, your input is valuable to us. Please feel free to contribute in the
following ways:
- **Issues and Pull Requests**: For specific questions, issues, or suggestions for improvements,
  open an issue or a pull request in this repository.
- **Mattermost Community**: Join the discussion in the
  [Integrations and Apps](https://community.mattermost.com/core/channels/integrations) channel
  on the Mattermost Community server.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Contact

For questions, feedback, or contributions regarding this project, please use the following methods:
- **Issues and Pull Requests**: For specific questions, issues, or suggestions for improvements,
  feel free to open an issue or a pull request in this repository.
- **Mattermost Community**: Join us in the Mattermost Community server, where we discuss all
  things related to extending Mattermost. You can find me in the channel
  [Integrations and Apps](https://community.mattermost.com/core/channels/integrations).
- **Social Media**: Follow and message me on Twitter, where I'm
  [@jlandells](https://twitter.com/jlandells).