---
title: Install AzuraCast
sidebar: auto
---

# Install AzuraCast

[[toc]]

AzuraCast is flexible and works on a broad number of environments, from inexpensive VPSes and servers to your own home computer running Windows, MacOS or Linux.

## Recommended Hosting Providers

### DigitalOcean 1-Click Install

We've partnered with our friends at [DigitalOcean](https://m.do.co/c/21612b90440f) to create a 1-Click installer that sets up and configures a droplet with AzuraCast's recommended Docker installation configured and ready to go.

Learn more about the 1-Click installer and create a droplet today [on the DigitalOcean Marketplace](https://marketplace.digitalocean.com/apps/azuracast).
m
### Linode

If you are hosting your installation with Linode, you can take advantage of community-maintained StackScripts to install AzuraCast.

1) From the management page, click "Create", then select "New Linode".
2) Switch to the "One-Click" tab at the top, then switch to the "Community StackScripts" secondary tab.
3) Enter "azuracast" in the search box.
4) Choose from either "azuracast-docker" (the recommended Docker installation) or "azuracast" (the non-Docker Ansible installation).

## Self-Hosted Installation

AzuraCast is powered by Docker and uses pre-built images that contain every component of the software. Don't worry if you aren't very familiar with Docker; our easy installer tools will handle installing Docker and Docker Compose for you, and updates are very simple.

::: warning
Some hosting providers use OpenVZ or LXC, and sometimes these technologies are incompatible with Docker. If the Docker installation does not work on your host, you should consider using a different server for AzuraCast, or you can use the unsupported [Ansible installation method](./install_ansible.html).
:::

### System Requirements

- A 64-bit x86 (x86_64) CPU (For ARM64 devices, like the Raspberry Pi 3/4, visit our [ARM64/Raspberry Pi guide](./install_rpi.html).)
- 1GB or greater of RAM 
- 20GB or greater of hard drive space

For Linux hosts, the `sudo`, `curl` and `git` packages should be installed before installing AzuraCast. Most Linux distributions include these packages already.

::: tip
**You don't need to install Docker or Docker Compose yourself; the AzuraCast installer handles both for you.**

It's recommended to use AzuraCast's installer, as it will automatically install the latest version of Docker and Docker Compose, which may be newer than the version that ships with your host operating system. You **should not** use the Ubuntu Snap installer to install Docker or Docker Compose, as this causes unexpected issues and is not supported.
::: 

### Installing

Connect to the server or computer you want to install AzuraCast on via an SSH terminal. You should be an administrator user with either root access or the ability to use the `sudo` command.

Pick a base directory on your host computer that AzuraCast can use. If you're on Linux, you can follow the steps below to use the recommended directory:

```bash
mkdir -p /var/azuracast
cd /var/azuracast
```

Use these commands to download our Docker Utility Script, set it as executable and then run the Docker installation process:

```bash
curl -fsSL https://raw.githubusercontent.com/AzuraCast/AzuraCast/master/docker.sh > docker.sh
chmod a+x docker.sh
./docker.sh install
```

On-screen prompts will show you how the installation is progressing.

Once installation has completed, be sure to follow the [post-installation steps](#post-installation-setup). You can also [set up LetsEncrypt](/developers/docker_sh.html#available-commands) or make other changes to your installation using the [Docker Utility Script](/developers/docker_sh.html#download-the-utility-script) that you've just downloaded.

::: tip
Want to further customize your installation? Check out our [support guide](/help/faq_docker.html) for some common examples, including custom port mappings and setting up SFTP access.
:::

### Post-Installation Setup

Once installation is complete, you should immediately visit your server's public web address. This may be the IP of the server, a domain name (if you've registered one and pointed it at the server), or `localhost` if you're running AzuraCast on your personal computer.

The initial web setup consists of the following steps:
1. Creating a "Super Administrator" account with system-wide administratration permissions
2. Creating the first radio station that the system will manage
3. Customizing important AzuraCast settings, like the site's base URL and HTTPS settings

Don't worry if you aren't sure of these items yet; you can always make changes to any of the items after setup is complete.

### Updating

Using the included Docker utility script, updating is as simple as running:

```bash
./docker.sh update-self
./docker.sh update
```

By default, the updater will prompt you to update your `docker-compose.yml` file. If you aren't making any changes to this file and want to automate the update process, you can use the command below to automatically answer "yes" to this question:

```bash
./docker.sh update-self && echo "y" | ./docker.sh update
```

### Backup and Restore

You can back up your AzuraCast installation without interrupting your stations by visiting the "Backup" page in System Administration, or by running from the command line:

```bash
./docker.sh backup path-to-backup.zip
```

To restore the backup later, run the following command:

```bash
./docker.sh restore path-to-backup.zip
```
