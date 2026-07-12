> Ansible role that scaffolds the control-machine workspace used by every other `apigee-opdk-*` playbook — directory tree, `ansible.cfg`, starter property templates, and clone of configuration/inventory sample repos.

> [!NOTE]
> For the skills demonstrated by this role and how they relate to the broader Apigee OPDK automation suite, see [SKILLS-ASSESSMENT.md](./SKILLS-ASSESSMENT.md).

## What the role actually does

> 🔄 **Evolution note:** The automation approach from this OPDK-era role has been consolidated into the `apigee-hybrid-workspace` Ansible collection. See the successor capability in the portfolio hub: [`carlosfrias/apigee-hybrid-workspace`](https://github.com/carlosfrias/apigee-hybrid-workspace) → `bap_coe/private_cloud/` and `bap_coe/apigee_hybrid/`. The collection README explains each role group’s business value and production context.


1. **Installs OS packages** (`git`, `rsync`, `tree`, `ansible`) via `yum` (Red Hat) or `apt` (Debian).
> [!NOTE]
> Engineering portfolio note — this project demonstrates Ansible controller scaffolding for the Apigee OPDK framework. See the [skills assessment →](SKILLS-ASSESSMENT.md) for the expertise applied.
2. **Creates the workspace directory tree** under `~/.apigee-workspace` (configurable via `ansible_workspace` / `apigee_workspace`):

   ```
   apigee-workspace/
   ├── ansible/
   │   ├── inventory/
   │   │   ├── aio/
   │   │   └── templates/
   │   ├── library/
   │   ├── logs/
   │   ├── multi-planet-configurations/
   │   │   ├── aio.cfg
   │   │   └── templates/
   │   ├── roles/
   │   └── tmp/
   ├── ansible.cfg
   ├── apigee/
   │   └── custom-properties.yml
   ├── apigee-secure/
   │   ├── credentials.yml
   │   └── license.txt
   └── playbooks/
   ```

3. **Copies starter templates** from `files/` into `apigee/` and `apigee-secure/` — 25+ property files covering mirror archives, yum repos, OpenLDAP, Qpid, SSO, SMTP, virtual hosts, Cassandra, Postgres, and more. Will not overwrite existing files (`force: no`).
4. **Generates `ansible.cfg`** from `templates/ansible.cfg.j2` with fact caching (JSON file), SSH pipelining, inventory path, and role/library paths all pointing into the workspace.
5. **Clones sample repositories** (`apigee-opdk-ansible-configuration-samples`, `apigee-opdk-ansible-inventory-samples`) via HTTPS or SSH into the workspace's `templates/` folders, providing ready-made inventory and configuration starters for single-node (AIO) and multi-planet deployments.
6. **Caches key paths** as Ansible facts (`ansible_workspace`, `apigee_secure_folder`, `apigee_custom_properties_folder`) so downstream roles can reference them without hard-coding.

## Role variables (selected)

| Variable | Default | Purpose |
|---|---|---|
| `remote_user` | *(required)* | SSH user written into `ansible.cfg` |
| `ansible_workspace` | `~/.apigee-workspace/ansible` | Root of the Ansible working directory |
| `apigee_workspace` | `~/.apigee-workspace` | Root of the Apigee configuration directory |
| `checkout_type` | `https` | `https` or `ssh` — controls which Git endpoint is used for sample repos |
| `pip_index_url` | *(undefined)* | When set, generates a `pip.conf` pointing to a private PyPI mirror |
| `private_key_file` | *(undefined)* | Path to SSH private key, injected into `ansible.cfg` |
| `template_repos` | 2 repos (configuration + inventory samples) | List of `{dir, repo_name, dest_name}` dicts to clone into the workspace |
| `os_packages` | `git`, `rsync`, `tree`, `ansible` | System packages the role ensures are present |

## Usage

```yaml
- name: Setup the controller workspace
  hosts: localhost
  connection: local
  gather_facts: no

  vars:
    remote_user: friasc

  roles:
    - { role: apigee-opdk-setup-ansible-controller }
```

For SSH-based repo cloning:

```yaml
vars:
  remote_user: friasc
  checkout_type: ssh
  private_key_file: ~/.ssh/id_ed25519
```

## Provenance

Originally authored by **Carlos Frias** as part of the Apigee Edge Private Cloud (OPDK) Ansible automation framework. Developed and maintained during tenure at Google / Apigee.

## License

Apache 2.0 — see [LICENSE](./LICENSE).

<!-- BEGIN Google Required Disclaimer -->
This is not an officially supported Google product.
<!-- END Google Required Disclaimer -->