# BeyondTrust Secrets Safe Ansible Playbook

Ansible playbook that retrieves managed account passwords and secrets from BeyondTrust Password Safe using the `beyondtrust.secrets_safe` lookup plugin.

## Prerequisites

- Ansible core >= 2.14
- Python >= 3.11
- BeyondInsight and Password Safe >= 23.1

### Install Ansible collections

```bash
ansible-galaxy collection install beyondtrust.secrets_safe beyondtrust.password_safe
```

### Install Python dependencies

```bash
pip install "beyondtrust-bips-library>=2.0.0,<3.0.0" "requests>=2.32.4" "urllib3>=2.6.0"
```

## Configuration

Export the following environment variables before running the playbook:

```bash
export PASSWORD_SAFE_API_URL="https://your-beyondinsight-host/BeyondTrust/api/public/v3"
export PASSWORD_SAFE_CLIENT_ID="your_client_id"
export PASSWORD_SAFE_CLIENT_SECRET="your_client_secret"
```

### Authentication options

The lookup plugin supports three authentication methods:

| Method | Required variables |
|---|---|
| OAuth (client credentials) | `client_id`, `client_secret` |
| API key | `api_key` (format: `<key>;runas=<user>`) |
| Client certificate | `certificate_path`, `certificate_password` |

## Usage

Update `managed_accounts_to_get` and `secrets_list` in `book.yml` to match your environment, then run:

```bash
ansible-playbook book.yml
```

Add `-v` through `-vvvv` for increasing verbosity. Be cautious with verbose output as it may expose secrets.

## Retrieval types

- **MANAGED_ACCOUNT** - retrieves managed account passwords. Path format: `system_name/account_name`
- **SECRET** - retrieves Secrets Safe credentials/files. Path format: `folder/secret_title`

Multiple items can be retrieved by comma-separating paths, e.g. `folder1/secret1,folder2/secret2`.
