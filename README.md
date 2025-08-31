# apt-cacher-ng Ansible Playbooks
 This repository provides ready-to-use Ansible playbooks to deploy docker apt-cacher-ng and configure client servers to utilize the cache server.

> **Info: Installs and configures apt-cacher-ng server in Docker, sets up clients to use the proxy, and disables caching for repository keys and signatures to avoid conflicts.**

---

## Quick Start

### Deploy the apt-cacher-ng server (Docker)

`ansible-playbook -i apt-cacher-server/inventory/prod apt-cacher-server/playbook.yml -kK --diff`

### Configure apt clients to use the proxy

`ansible-playbook -i apt-cacher-client-config/inventory/prod apt-cacher-client-config/playbook.yml -kK --diff`

---

## How It Works

- The **server playbook** provisions an apt-cacher-ng proxy inside a Docker container, exposing it to your network.
- The **client playbook** deploys apt proxy configuration to multiple machines, so their package manager traffic is routed through your caching proxy.
- Configuration additions in `acng.conf.j2` ensure apt-cacher-ng does **not cache repository signatures or keys** to prevent update/verification conflicts:

DontCacheResolved: .*InRelease</br>
DontCacheResolved: .*Release</br>
DontCacheResolved: .*gpg</br>
DontCacheResolved: .*sig</br>


---

## Repository Structure

- `apt-cacher-server/` — files for the apt-cacher-ng Docker server.
- `apt-cacher-client-config/` - client config templates.

---

## Notes

- After editing proxy configuration or server settings, restart the apt-cacher-ng Docker container for changes to take effect.
- Make sure your inventory files match your infrastructure.
- For troubleshooting and questions, please open an issue or contact the repository maintainer.

---

Thank you for using these playbooks to speed up, cache, and secure your Debian/Ubuntu package management!
