# ansi-role-buildserv

An Ansible role for Ubuntu 24.04 (`noble`) that sets up a Rust build server. It compiles:
1. **Arexibo** inside an isolated Ubuntu 24.04 Podman container (keeping heavy C++/Qt6 build dependencies off the host).
2. **Static `x86_64-unknown-linux-musl` Rust CLI tools** natively on the host via `cargo` and `musl-gcc`.

## Requirements

- Target host running Ubuntu 24.04 (`noble`)
- Ansible >= 2.10
- Updated `apt` cache prior to running (the role sets `update_cache: false`)
- `ansible_user` defined for file ownership

## Role Variables

Defined in `defaults/main.yml`:

| Variable | Default | Description |
| :--- | :--- | :--- |
| `build_server_base_dir` | `"/opt/rust_builds"` | Base directory where `arexibo/`, `musl_tools/`, and `container/` are created. |
| `arexibo_force_rebuild` | `false` | When `true`, forces a fresh clone and release build of Arexibo even if the binary already exists. |
| `rust_musl_tools` | *(see below)* | List of crates to compile statically into `{{ build_server_base_dir }}/musl_tools/bin/`. |

Default `rust_musl_tools` list:

```yaml
rust_musl_tools:
  - { name: 'ripgrep', bin: 'rg', version: '15.1.0' }
  - { name: 'fd-find', bin: 'fd', version: '10.4.2' }
  - { name: 'zoxide', bin: 'zoxide', version: '0.9.9' }
  - { name: 'bat', bin: 'bat', version: '0.26.1' }
  - { name: 'du-dust', bin: 'dust', version: '1.2.4' }
  - { name: 'procs', bin: 'procs', version: '0.14.11' }
  - { name: 'git-delta', bin: 'delta', version: '0.19.2' }
  - { name: 'dysk', bin: 'dysk', version: '3.6.1' }
  - { name: 'eza', bin: 'eza', version: '0.23.4' }
  - { name: 'code2prompt', bin: 'code2prompt', version: '4.3.0' }