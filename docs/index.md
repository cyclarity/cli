# Clarity CLI 🚀
Interactive command-line tool for test management, profile handling, and component uploads to **CyClarity**.

> **Why another CLI?**  
> Clarity CLI bundles everyday actions (login, test execution, artifact upload) into a single, script-friendly interface—so you can automate your QA pipelines without juggling multiple tools.

---

## 📦 Installation

```bash
pip install clarity-cli
# or, from source:
git clone https://github.com/your-org/clarity-cli && cd clarity-cli
pip install -e .
```

---

## ⚡ Quick start

```bash
# 1️⃣  Configure a profile (once)
clarity profile-setup   --profile dev   --client-id <ID>   --client-secret <SECRET>   --token-endpoint https://auth.example.com/oauth/token   --scope "clarity offline_access"   --project 42 --workspace 7 --agent-id AGENT_1   --default

# 2️⃣  Log in (refreshes & stores JWT)
clarity login --profile dev

# 3️⃣  Run a single test
clarity execute --test-id TEST-123
```

---

## 🗂 Config files & profiles

| File / Dir | Purpose | Default location |
|------------|---------|------------------|
| `config.json` | Stores profiles & tokens | `~/.clarity/config.json` |
| `~/.clarity/` | Root config directory (auto-created) | – |

> Profiles let you switch between environments (dev, staging, prod) without re-entering secrets.

---

## 🛠 Command reference

### `clarity execute`

Run a test interactively or headless.

| Option | Description | Example |
|--------|-------------|---------|
| `-t, --test-id` | Execute a specific test immediately. | `-t TEST-123` |
| `-p, --profile` | Profile to use (falls back to *default*). | `-p prod` |
| `-c, --override-config-path` | Use an alternative `config.json`. | `-c ./tmp/config.json` |
| `--project` | Override default project ID. | `--project 99` |
| `--workspace` | Override default workspace ID. | `--workspace 5` |
| `--agent-id` | Run on a specific agent. | `--agent-id AGENT_2` |
| `--parameters-file` | JSON file with flow variable values. | `--parameters-file vars.json` |

```bash
clarity execute -t TEST-123 --parameters-file vars.json
```

---

### `clarity profile-setup`

Create or update a profile.

| Option | Required? | Description |
|--------|-----------|-------------|
| `--profile, -p` | ✔ | Profile name (e.g. `prod`). |
| `--client-id, -id` | ✔ | OAuth client ID. |
| `--client-secret, -cs` | ✔ | OAuth client secret. |
| `--token-endpoint, -e` | ✔ | OAuth token URL. |
| `--scope, -s` | ✔ | Space-separated OAuth scopes. |
| `--project, --workspace, --agent-id` | – | Default IDs for operations. |
| `--domain` | – | Override Clarity domain (multi-tenant). |
| `--default, -d` | – | Mark as the default profile. |

```bash
clarity profile-setup -p staging -id XXX -cs YYY -e https://auth/staging/token -s "clarity"
```

---

### `clarity login`

Refresh and cache an access token.

| Option | Description |
|--------|-------------|
| `-p, --profile` | Profile to log in with (defaults to *default*). |
| `-c, --override-config-path` | Alternate `config.json` location. |

```bash
clarity login -p prod
```

---

### `clarity upload`

Package & upload a component.

| Option | Description |
|--------|-------------|
| `--component-path` | Path to the Poetry project (defaults to CWD). |
| `-e, --entrypoint` | `module:function` entrypoint. |
| `-r, --running-env` | Target runtime (`IOT` or `CLOUD`). |
| `--pyc` | Compile to `*.pyc` before upload. |
| `-y` | Non-interactive “yes to all”. |

```bash
clarity upload   --component-path ./my_component   --entrypoint my_component.main:handler   -r CLOUD --pyc -y
```

---

## 🤖 Verbose mode

Add `-v` after `clarity` (before sub-commands) to print debug logs:

```bash
clarity -v execute -t TEST-123
```


---

*Last updated: 05-06-2025*
