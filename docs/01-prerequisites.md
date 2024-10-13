# Prerequisites

In this lab you will review the machine requirements necessary to follow this tutorial.

## Virtual or Physical Machines

This tutorial requires four (4) virtual or physical ARM64 machines running Debian 12 (bookworm). The follow table list the four machines and thier CPU, memory, and storage requirements.

| Name    | Description            | CPU | RAM   | Storage |
|---------|------------------------|-----|-------|---------|
| jumpbox | Administration host    | 1   | 512MB | 10GB    |
| server  | Kubernetes server      | 1   | 2GB   | 20GB    |
| node-0  | Kubernetes worker node | 1   | 2GB   | 20GB    |
| node-1  | Kubernetes worker node | 1   | 2GB   | 20GB    |

How you provision the machines is up to you, the only requirement is that each machine meet the above system requirements including the machine specs and OS version. Once you have all four machine provisioned, verify the system requirements by running the `uname` command on each machine:

```bash 
uname -mov
```

After running the `uname` command you should see the following output:

```text
#1 SMP Debian 6.1.55-1 (2023-09-29) aarch64 GNU/Linux
```

You maybe surprised to see `aarch64` here, but that is the official name for the Arm Architecture 64-bit instruction set. You will often see `arm64` used by Apple, and the maintainers of the Linux kernel, when referring to support for `aarch64`. This tutorial will use `arm64` consistently throughout to avoid confusion.

Next: [setting-up-the-jumpbox](02-jumpbox.md)

## Addition: Vagrant Based Setup

```
mkdir k8s-vagrant
vim Vagrantfile
```

Add this:

```ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :

#Place Vagrantfile in the directory you run vagrant from.
#This should also contain ubuntu.yml which configure VMs

# setting for all VMs
Vagrant.configure("2") do |config|
  config.vm.provider "virtualbox" do |v|
    v.memory = 2048
    v.cpus = 1
  end

  config.vm.define "jumpbox" do |jumpbox|
    jumpbox.vm.box = "debian/bookworm64"
	jumpbox.vm.hostname = "jumpbox"
	jumpbox.vm.network "private_network", ip: "192.168.60.5"
  end

  config.vm.define "server" do |server|
    server.vm.box = "debian/bookworm64"
    server.vm.hostname = "server"
    server.vm.network "private_network", ip: "192.168.60.10"
  end

  config.vm.define "node0" do |node0|
    node0.vm.box = "debian/bookworm64"
    node0.vm.hostname = "node-0"
    node0.vm.network "private_network", ip: "192.168.60.11"
  end

  config.vm.define "node1" do |node1|
    node1.vm.box = "debian/bookworm64"
    node1.vm.hostname = "node-1"
    node1.vm.network "private_network", ip: "192.168.60.12"
  end

end
```
