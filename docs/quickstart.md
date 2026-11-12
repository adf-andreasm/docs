---
icon: lucide/rocket
---

# Quickstart Guide

## Install system dependencies
## Installing uv
The main build tool used by the Potos iso builder project is uv.
It is recomandet to install it system-wide.

Follow the [offical uv installation guide](https://docs.astral.sh/uv/getting-started/installation) or if running on fedora install it with:
```bash
sudo dnf install uv
```

## Installing docker

Follow the [offical docker installation guide](https://docs.docker.com/engine/install) and add your user to the docker group.

## Installing libvirt


## Clone project
```bash
git clone https://github.com/projectpotos/potos-iso-builder.git
```
```bash
potos-iso-builder.git
```
```bash
(cd tests/; mv output output.bak; ln -s ../output .)
```

## Install local project dependencies
```bash
uv sync
```
```bash
uvx --from go-task-bin task
```
```bash
uv tool install virt-firmware
```

## Preparing minimal Potos config
Copy the example configuration
```bash
mkdir input
cp examples/minimal/config.yml input
```

### Set keyboard layout
Set a keyboard layout in input/config.yml that the iso unstallation runs automatically trough
```yaml
locale:
  lang: "en_US.UTF-8"
  keyboard: "ch"
  timezone: "UTC"
```

### Admin Password
Generate a password for the potos admin user
```bash
openssl passwd -6
```
Replace password_hash in input/config.yml
```yaml
initial_user:
  username: "admin"
  password_hash: <your_hash>
```

## Build minimal Potos iso
```bash
uvx --from go-task-bin task build
```

```bash
uvx --from go-task-bin task test:bootstrap
```
