# DRAFT - ojuny-pro-installer

Status: draft scope. Nothing is built. This file describes what the repository
is supposed to contain, not what it contains today.

## Purpose

The install media for Ojuny Pro. It installs the Pro template by default, but
it keeps the generic installer behavior so an organization can point it at any
Ojuny template.

It is the workplace image: guided install and security choices made explicit.
Pro is individualized and has no Fleet enrollment path.

## What the repository will contain

```text
ojuny-pro-installer/
  flake.nix                  ISO and app outputs
  nixos/                     live profiles for each variant
  installer/                 UI and install orchestration
    presets/                 GNOME layout preset selection
    security/                encryption and Secure Boot choices
  docs/                      install guide and security choices
  tests/                     VM and hardware checklists
```

### Install media

- Attended online ISO.
- Attended offline ISO with the full Pro closure.
- Attended discovery ISO using local Ojuny cache peers.
- Unattended ISO with a declarative answer file.

The image reuses the install engine from the terminal installer. It does not
own a separate partitioning or user-generation path.

### Install flow

1. Choose the template. Pro is the default; another Ojuny template is allowed.
2. Choose the GNOME layout preset: default, Windows-like or macOS-like.
3. Choose disk, optional LUKS2 encryption and bootloader mode.
4. Create the administrator account and hostname.
5. Review the security baseline and any disabled control.
6. Install, then reboot into GDM.

### Security choices

- Encryption is presented as recommended, not silently forced.
- Secure Boot enrollment is offered after install, with the documented hardware
  limits.
- The install summary lists firewall, SSH, remote desktop and update policy.
- No telemetry, no cloud account and no remote management connection. Pro is
  not enrolled in Fleet.

### Documentation

- The install screens and the preset choices.
- What each security control does and its default.
- The offline and discovery variants and their disk requirements.
- Supported firmware and the boot order behavior.

### Tests

- Each ISO boots in QEMU.
- A Pro install reaches GDM with each layout preset.
- An offline install completes without network access.
- An attended install of a non-Pro Ojuny template succeeds.
- An encrypted install prompts correctly and boots.

## Limits and non-goals

- `x86_64-linux` only.
- No Wi-Fi in unattended installs.
- No NTFS resizing and no Windows BitLocker handling.
- No automatic Secure Boot enrollment.
- No Fleet server implementation and no Fleet enrollment. Fleet deploys its
  own `ojuny-endpoint` template.
- No custom Pro package repository.
- No promise of certification or enterprise support.

## Open questions

- Is the Pro installer graphical or terminal? The workplace target suggests a
  guided graphical flow, but the engine is terminal-first.
- Are layout presets fixed at install, or changeable only after first boot?
- Does the offline image include the professional application closure only, or
  also a local cache server?

## Sources

- `ojuny-template/docs/00-overview.md`, `01-architecture.md`,
  `03-host-setup.md`, `05-settings-gui-limitations.md`.
- `ojuny-installer/README.md` and `docs/attended-install.md`,
  `docs/offline-install.md`, `docs/unattended-install.md`,
  `docs/remote-deployment.md`.
- `nixos-configs/notes/ojuny pro - early draft.md`.
