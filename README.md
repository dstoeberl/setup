# setup

A small, opinionated Ansible playbook targeted at **Debian 13** to provision a personal GNOME workstation (Flatpaks, common dev tools). This repository is intended to be run directly on the local workstation (localhost) rather than from a separate control node.

## Summary ✅
- Purpose: reproducible, idempotent workstation setup using Ansible task fragments in `tasks/`.
- Controls are declarative: edit `config.yml` to enable/disable `features` and `apps`.
- Note: some features and documentation in this repository were developed with assistance from supervised AI; all edits are reviewed and approved by the repository maintainer.

---

## Quickstart — local (safe) ⚡
1. Install prerequisites (Debian 13 - preferred):

   ```bash
   sudo apt update
   sudo apt install -y ansible ansible-lint
   ansible-galaxy collection install community.general
   ```

2. Syntax-check and dry-run:

   ```bash
   ansible-playbook --syntax-check main.yml
   ansible-playbook main.yml -K --check --diff
   ```

3. Apply (local):

   ```bash
   ansible-playbook main.yml -K
   ```

> Note: this playbook is intended to be run directly on the target workstation (localhost). Some tasks require `become`/sudo — when running interactively you can pass `-K` to `ansible-playbook` to prompt for the privilege-escalation password (examples above). Run the playbook from the account you want provisioned.

---

## Configuration (how to change what gets installed) 🔧
- `config.yml` contains two top-level lists: `features` and `apps`.
  - Enable desktop features (example: `gnome`, `workstation`).
  - Enable individual apps (example: `steam`, `waydroid`).
- The playbook conditionally imports the corresponding `tasks/*.yml` fragments.

Example (current):
```yaml
features:
  - gnome
  - workstation

apps:
  - calibre
  - spotify
  - virtualbox
  - waydroid
```

---

## Extending the repo
- To add a feature: create `tasks/feature-name.yml` and import it from `main.yml` with a when-clause.
- To add an app: add toggles to `config.yml` and a `tasks/app-*.yml` that installs it (apt/flatpak/pipx).

---

## Troubleshooting 🛠️
- `ansible-lint` not found → preferred: `sudo apt install ansible-lint` on Debian 13.
- Flatpak installs failing for user vs system → check `method: user` vs `method: system` and run with the appropriate privileges.
- GCM download hard-coded to a release asset — it may need updating if upstream renames assets.

---

## TODO / roadmap
- Add `requirements.yml` for pinned collections
- Add automated test (molecule or smoke VM)

---

## Acknowledgements
Some features and documentation in this repository were developed with assistance from supervised AI; all such contributions are reviewed and approved by the repository maintainer.

---

## License
This repository is licensed under the MIT License. See `LICENSE`.
