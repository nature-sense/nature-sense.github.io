---
title: Installing the Trap Software
parent: Software Installation
nav_order: 3

---

# Installing the Trap Software

The Trap software is written in python and it available in the github repository:

[https://github.com/nature-sense/yolo-trap-py.git](https://github.com/nature-sense/yolo-trap-py.git)

The software in the trap is simple a clone of this repository, and it is created as follows:

Firstly ssh into the trap e.g

```shell
ssh trap@mytrap.local
trap@mytrap.local's password:
```

Then issue the following commands

```shell
sudo apt install -y git
git config --global init.defaultBranch main
git init
git remote add origin  https://github.com/nature-sense/yolo-trap-py.git
git pull origin main
```

This will have created a clone of the github repository on the trap.

The final step is to run the install script which will load all required software, and configure the system.

```shell
./install.sh
```

This will take a few minutes.

Finally reboot the sysyem

```shell
sudo reboot
```

On restart the trap should be visible in the AI Trap phone app:

![mytrap](../images/mytrap.png)
