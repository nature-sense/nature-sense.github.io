---
title: Installing the Trap Software
parent: Software Installation
nav_order: 3

---

# Installing the Trap Software

The Trap software is written in python and is available in the GitHub repository:

[https://github.com/nature-sense/ai-trap-py.git](https://github.com/nature-sense/ai-trap-py.git)

The software in the trap is simply a clone of this repository. The respository contains an installation script, so the steps to install it are simply:

- Create a clone of the software on the trap.
- Run the install script.

Thereafter the software can be updated by simply pulling the latest version from GitHub.

To install the software ssh into the trap e.g

```shell
ssh trap@mytrap.local
trap@mytrap.local's password:
```

Then issue the following commands

```shell
sudo apt install -y git
git config --global init.defaultBranch main
git init
git remote add origin  https://github.com/nature-sense/ai-trap-py.git
git pull origin main
git branch --set-upstream-to=origin/main main
```

This will have created a clone of the github repository on the trap.

Now run the install scrip, this will load all the additional required software and configure the system.

```shell
./install.sh
```

This will take a few minutes.

Finally reboot the sysyem

```shell
sudo reboot
```



# Updating the Software
