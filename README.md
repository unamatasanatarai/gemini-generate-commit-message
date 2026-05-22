# Gemini Generate Commit Message

![Language](https://img.shields.io/badge/language-bash-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Requirements](https://img.shields.io/badge/dependencies-git%20|%20curl%20|%20jq-orange)
![Interface](https://img.shields.io/badge/interface-CLI-lightgrey)
![Platform](https://img.shields.io/badge/platform-POSIX-brightgreen)
![API](https://img.shields.io/badge/API-Gemini-red)

A set of Bash utilities designed to automate Git commit message generation using the Google Gemini API. The tool analyzes staged changes, filters out noise, and interacts with the Gemini API to construct a structured, concise commit message following Conventional Commits guidelines.

---

## Features

* **Conventional Commits Enforcement**: Generates standardized, single-line messages (max 256 characters) matching standard scopes like `feat`, `fix`, `refactor`, and `docs`.
* **Automatic Noise Filtering**: Automatically excludes standard dependency lockfiles (`package-lock.json`, `pnpm-lock.yaml`, `yarn.lock`, `bun.lockb`) from the diff to optimize API context usage.
* **Context Truncation**: Truncates large Git diffs at 90,000 characters to manage token boundaries safely.
* **Interactive Mode**: Includes an interactive workflow script (`ggic`) that automatically stages all changes, requests the generated message, displays a preview, and prompts for confirmation before finalizing the Git commit.

---

## Tech Stack

* **Shell Scripting**: Bash 
* **JSON Processing**: `jq` 
* **Networking**: `curl` 
* **Version Control**: Git 

---

## Project Structure

```text
gemini-generate-commit-message    Core script that handles diff extraction, API token processing, and payload delivery 
ggic                              Interactive wrapper script to stage changes, preview messages, and execute commits 
install.sh                        Automated installation script for fetching and configuring the binaries
LICENSE                           MIT License file 
```

---

## Configuration

The scripts rely on the XDG Base Directory Specification for handling data and configurations.

### Authentication

The Gemini API key must be supplied using one of two methods:

1. 
**Environment Variable**: Export `GEMINI_API_KEY` directly into your environment.


2. 
**Configuration File**: Store the plaintext key in `$XDG_CONFIG_HOME/gcg/key` (defaults to `~/.config/gcg/key`).



### Model Customization

By default, the script calls `gemini-3.1-flash-lite`. You can modify this behavior by setting the following variable:

* 
`GIT_SUMMARY_MODEL`: Set this environment variable to target an alternative Gemini model layout.

* To list out available models run: `gemini-show-models`


---

## Installation

An installation script is provided to download the tools directly into your user binaries folder (`$XDG_BIN_HOME` or `~/.local/bin`).

```bash
curl -sSL https://raw.githubusercontent.com/unamatasanatarai/gemini-generate-commit-message/master/install.sh | bash
```

### Path Requirements

If your local bin path is not listed in your shell profile, add it manually:

```bash
export PATH="$HOME/.local/bin:$PATH"
```

---

## Usage

### Interactive Commit Workflow

Run the wrapper utility within any active Git repository to stage changes and confirm an AI-generated message interactively:

```bash
ggic
```

### Direct Message Generation

Run the core generator directly to return a raw string output based purely on currently staged (`git add`) changes:

```bash
gemini-generate-commit-message
```

---

## License

This project is licensed under the terms of the [MIT License](LICENSE).

