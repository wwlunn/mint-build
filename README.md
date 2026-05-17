# Linux Mint application installer

Ansible playbook for setting up regular applications on a fresh Linux Mint machine.

## Usage

Install Ansible first:

```bash
sudo apt update
sudo apt install ansible
```

Edit the application list in `vars/apps.yml`, then run:

```bash
ansible-playbook -i inventory.ini site.yml --ask-become-pass
```

## App sources

- `vmware_guest_packages` is installed immediately after the first APT cache refresh and before the package upgrade.
- `apt_upgrade` runs an APT package upgrade after refreshing the package cache.
- `apt_packages` installs packages from the Linux Mint/Ubuntu repositories.
- `install_go` installs the latest official Go Linux archive from `go.dev` into `/usr/local/go`.
- `install_docker` installs Docker Engine, Buildx, and the Compose plugin from Docker's official Ubuntu APT repository.
- `third_party_apt_repositories` adds vendor repositories, then installs their packages. Set `enabled: true` on the entries you want.
- `deb_packages` downloads and installs standalone `.deb` packages.
- `install_nvm` installs or updates nvm for the user running `ansible-playbook`. It resolves the latest release from GitHub at run time and executes the upstream installer with `curl`.
- `install_latest_node` installs the latest Node.js release through nvm and can set it as the default nvm version.
- `npm_global_packages` installs or updates global npm packages through the nvm-managed `npm`. The default list includes Codex CLI and Claude Code.
- `install_zed` installs or updates the latest Zed editor using the official Linux installer from `https://zed.dev/install.sh`.
- `zed_settings` writes Zed preferences to `~/.config/zed/settings.json` for `zed_user`. The default uses a dark theme and VS Code keymap.
- `pin_cinnamon_panel_apps` appends entries from `cinnamon_panel_pinned_apps` to the Cinnamon Grouped Window List panel pins for `cinnamon_panel_user`. The default pins Zed and Google Chrome.
- `install_uv` installs or updates Astral uv for Python package and project management.

For Steam, Wine, or other software that needs 32-bit libraries, set `enable_i386_arch: true` in `vars/apps.yml`.

After the playbook adds your user to the `docker` group, log out and back in before running Docker without `sudo`.
