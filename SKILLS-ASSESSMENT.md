> **Skill domain:** Infrastructure-as-Code / Ansible control-plane bootstrapping / Private-cloud automation scaffolding

## Why this role is notable

This is the **foundation role** for the entire 150+ role Apigee OPDK Ansible framework — the first role every playbook depends on to establish a consistent, reproducible control-machine environment. Rather than assuming a pre-configured workstation, it programmatically scaffolds the complete directory tree, generates a tuned `ansible.cfg`, seeds 25+ domain-specific property templates, and clones shared configuration/inventory samples from Git. Without this role, none of the downstream `apigee-opdk-*` roles have a workspace to write to or facts to reference. It also embodies an opinionated multi-planet deployment model where a single controller can manage separate dev / test / staging / prod inventory trees side-by-side, each with its own `ansible.cfg` variant.

## Expertise demonstrated

- **Ansible role architecture** — designing a role that produces its own consumer environment (workspace, config, facts) rather than consuming one; idempotent directory creation with `force: no` on starter templates to avoid clobbering existing customizations.
- **Private-cloud deployment topology** — encoding the multi-planet pattern (separate inventory trees per environment) directly into the workspace layout, reflecting real Apigee Edge Private Cloud customer deployment patterns.
- **Configuration templating at scale** — shipping 25+ Jinja2/YAML property templates covering every Apigee OPDK subsystem (Cassandra, OpenLDAP, Qpid, Postgres, SSO, SMTP, virtual hosts, yum repos, mirror archives, etc.) as starter files that operators customize per-environment.
- **Cross-platform package management** — dual `yum`/`apt` task branches gated by `ansible_os_family`, enabling the same role to bootstrap controllers on both RHEL and Debian derivatives.
- **Git-based configuration distribution** — parameterized HTTPS/SSH checkout of companion sample repos, letting operators choose their authentication method while maintaining a single role interface.
- **Ansible fact injection** — caching workspace paths as `set_fact` with `cacheable: yes` so downstream roles can reference `ansible_workspace`, `apigee_secure_folder`, and `apigee_custom_properties_folder` without re-deriving them.
- **Performance tuning defaults** — generating an `ansible.cfg` with SSH pipelining, `ControlMaster`/`ControlPersist`, JSON fact caching, smart gathering, and `forks = 10` — a production-ready baseline for managing large Apigee clusters.

## How this shows the expertise

| Capability | Evidence |
|---|---|
| Control-plane bootstrapping | `tasks/main.yml` creates the full workspace tree, generates `ansible.cfg` from template, and caches paths as facts — all in a single idempotent run |
| Multi-planet deployment model | `defaults/main.yml` defines `multi-planet-configurations/` with per-environment `ansible.cfg` variants; `template_repos` clones configuration and inventory samples that demonstrate AIO and multi-region layouts |
| Domain-specific property seeding | `files/` ships 25+ starter property files (mirror, yum, OpenLDAP, Qpid, SSO, SMTP, Cassandra, Postgres, virtual hosts, etc.) copied with `force: no` |
| Cross-platform OS prep | `tasks/main.yml` conditional `yum`/`apt` blocks gated by `ansible_os_family` |
| Configurable auth for Git | `checkout_type` variable switches between `repository_secure_endpoint_ssh` and `repository_secure_endpoint_https` |
| Fact-based path sharing | `set_fact` with `cacheable: yes` publishes `ansible_workspace`, `apigee_secure_folder`, `apigee_custom_properties_folder` |
| Performance baseline | `templates/ansible.cfg.j2` enables SSH pipelining, `ControlPersist`, JSON fact caching, smart gathering, `forks = 10` |

## Related expertise

| Repository | Relevance |
|---|---|
| [apigee-opdk-setup-ansible-controller-backup](https://github.com/carlosfrias/apigee-opdk-setup-ansible-controller-backup) | Backup/restore companion for this controller workspace |
| [apigee-opdk-playbook-setup-ansible](https://github.com/carlosfrias/apigee-opdk-playbook-setup-ansible) | Top-level playbook that invokes this role |
| [apigee-opdk-ansible-configuration-samples](https://github.com/carlosfrias/apigee-opdk-ansible-configuration-samples) | Sample Ansible configurations cloned by this role |
| [apigee-opdk-ansible-inventory-samples](https://github.com/carlosfrias/apigee-opdk-ansible-inventory-samples) | Sample inventory layouts cloned by this role |
| [apigee-opdk-setup-silent-installation-config](https://github.com/carlosfrias/apigee-opdk-setup-silent-installation-config) | Consumes the workspace paths this role creates to generate silent install config files |
| [apigee-opdk-setup-default-settings](https://github.com/carlosfrias/apigee-opdk-setup-default-settings) | Sets OPDK defaults that rely on the workspace this role scaffolds |
| [apigee-opdk-modules](https://github.com/carlosfrias/apigee-opdk-modules) | Custom Ansible modules (`library/`) that populate the `library/` directory this role creates |

## Provenance

Originally authored by **Carlos Frias** as part of the Apigee Edge Private Cloud (OPDK) Ansible automation framework. Developed and maintained during tenure at Google / Apigee.

## License

Apache 2.0 — see [LICENSE](./LICENSE).

<!-- BEGIN Google Required Disclaimer -->
This is not an officially supported Google product.
<!-- END Google Required Disclaimer -->