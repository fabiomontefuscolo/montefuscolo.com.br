---
title: "Port forwarding on two interfaces with pf"
date: 2024-06-04T21:11:33+02:00
summary: "Port forwarding on two interfaces with pf"
tags: ["python", "nvim", "neovim"]
draft: false
---

## The problem

I have a FreeBSD virtual machine with two interfaces, one set as a bridge and the other
as a internal network for virtual machines. I want both interfaces forwarding ports to
an ingress jail.

The interfaces are `em0` and `em1`, the bridge and internal network respectively.

## The solution

### Packet Filter (pf)

```
# /etc/pf.conf
ext_if="{ em0, em1 }"

set block-policy return
scrub in on $ext_if all fragment reassemble
set skip on lo

table <jails> persist
nat on $ext_if from <jails> to any -> $ext_if
rdr-anchor "rdr/*"
### Backing up your stuff
```

### Setting RDR rules on the ingress jail

The jail named `ingress` will be the one receiving the traffic.

```sh
# Format is:
#     bastille rdr TARGET tcp host_port jail_port

$ bastille rdr ingress tcp 80 80
$ bastille rdr ingress tcp 443 443
```
