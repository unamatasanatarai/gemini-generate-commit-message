# gemini-generate-commit-message

![Language](https://img.shields.io/badge/language-bash-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Dependencies](https://img.shields.io/badge/dependencies-git%20%7C%20curl%20%7C%20jq-orange)
![Interface](https://img.shields.io/badge/interface-CLI-lightgrey)
![Platform](https://img.shields.io/badge/platform-POSIX-brightgreen)
![API](https://img.shields.io/badge/API-Gemini-red)

A set of lightweight Bash utility scripts designed to automate Git commit message generation using the Google Gemini API. The utility analyzes staged changes, filters out common noise files, and interacts with the Gemini API to construct structured, concise commit messages following Conventional Commits guidelines.

---

## Features

- **Conventional Commits Enforcement**: Generates single-line messages (max 256 characters) with standardized scopes such as `feat`, `fix`, `refactor`, and `docs`.
- **Automatic Noise Filtering**: Excludes standard dependency lockfiles (`package-lock.json`, `pnpm-lock.yaml`, `yarn.lock`, `bun.lockb`) from the Git diff to optimize context tokens.
- **Diff Context Management**: Automatically truncates Git diff inputs at 90,000 characters to prevent API request limits from being exceeded.
- **Interactive Workflows**: Includes helper scripts (`ggic` and `ggicp`) to stage all working directory changes, generate and preview the AI-crafted message, and execute Git commits or remote pushes with confirmation.
- **Model Listing**: Queries available model endpoints dynamically using the API key to view available language model configurations.

---

## Tech Stack

- **Core Scripting**: Bash
- **Data Querying & Parsing**: `curl`, `jq`
- **Version Control**: Git
- **API Integration**: Google Gemini API (v1beta endpoint)

---

## Project Structure

```text
gemini-generate-commit-message   Core script containing diff parsing, token limiting, API payload generation, and response formatting.
gemini-show-models               Utility script that queries and lists available models from the Gemini API.
ggic                             Interactive wrapper script that runs 'git add --all', generates the commit message, and commits upon confirmation.
ggicp                            Interactive wrapper script that runs 'git add --all', generates the commit message, commits, and pushes to origin.
install.sh                       Download and installation script to place utilities into local bin directories and check PATH settings.
LICENSE                          MIT License declaration.
```

---

## Installation

An automated installation script is provided to download and configure the tools in your user bin directory.

### Standard Installation

Execute the installation script using `curl`:

```bash
curl -sSL https://raw.githubusercontent.com/unamatasanatarai/gemini-generate-commit-message/master/install.sh | bash
```

This downloads and configures the binaries inside your `$XDG_BIN_HOME` or `$HOME/.local/bin` folder.

### Environment Path Configuration

If your local bin directory is not listed in your shell profile, add the path manually:

```bash
export PATH="$HOME/.local/bin:$PATH"
```

---

## Configuration

The utility relies on the XDG Base Directory Specification for config and temporary storage.

### Authentication

You must provide a Gemini API key using one of the following methods:

1. **Environment Variable**: Export the key directly into your shell profile:
   ```bash
   export GEMINI_API_KEY="your_api_key_here"
   ```
2. **Plaintext File**: Save the key in `$XDG_CONFIG_HOME/gemini-commit-message/key` (which defaults to `~/.config/gemini-commit-message/key`).

### Model Customization

By default, the script calls `gemini-3.1-flash-lite`. You can customize the model by exporting the following environment variable:

```bash
export GIT_SUMMARY_MODEL="alternative-model-name"
```

To list out all available models for your API key, execute:

```bash
gemini-show-models
```

---

## Usage

### Interactive Stage and Commit

Automatically stages all changes, shows a preview of the generated message, and prompts for confirmation to commit:

```bash
ggic
```

### Interactive Stage, Commit, and Push

Stages all changes, creates a commit using the generated message, and pushes to the current branch on the remote:

```bash
ggicp
```

### Direct Message Output

Generates and prints a raw commit message to stdout based on currently staged changes without executing a commit:

```bash
gemini-generate-commit-message
```

---

## License

This project is licensed under the terms of the [MIT License](LICENSE).
