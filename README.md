# @indus/bot-core

Shared SDK for the INDUS bots. It is a library, not a service, so it has no systemd unit of its own.

What it owns:

- One SQLite lock in `INDUS_SHARED_DIR` so every bot shares a single local LLM call at a time.
- Ollama and OpenAI-compatible clients, plus a mock used by tests.
- Bot registration, SSO verification, approvals, the job queue, and the bot UI shell.
- The commander's allow-list. Tasks named publish, send, deploy, approve, restore, or apply-fix are rejected.

This folder is the `indus-bot-core` workspace from the [INDUS Ops monorepo](https://github.com/mafzalkalwardev/indus-ops) (imported from commit `fe1c8bf`). Install and deploy from that monorepo. It builds this library together with the rest of the services:

```bash
# from a checkout of https://github.com/mafzalkalwardev/indus-ops
npm install
sudo bash deploy/install-vps.sh
```

`npm install` at the monorepo root installs this workspace. `deploy/install-vps.sh` writes `/opt/indus` and enables the bot units. This package has no systemd unit and no nginx site of its own. Upgrades from the monorepo checkout:

```bash
sudo rsync -a --exclude node_modules --exclude data --exclude .env ./ /opt/indus/
sudo -u indus bash -lc 'cd /opt/indus && npm install'
sudo systemctl restart 'indus-*'
```

Other repos depend on this folder with `"@indus/bot-core": "file:../indus-bot-core"`. On the VPS they sit side by side under `/opt/indus`.

To test this library on its own:

```bash
npm install
npm test
```
