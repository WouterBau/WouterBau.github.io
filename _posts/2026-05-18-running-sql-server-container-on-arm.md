---
layout: post
title:  "Running SQL Server Container on ARM"
date:   2026-05-18
description: ""
tags:
 - SQL Server
 - Docker
share: true
---

https://github.com/containers/podman/issues/27078#issuecomment-3663860803

Recreate Podman machine specifying Apple Hypervisor (screenshots)

perform these commands

```bash
podman machine ssh podmachine-apple-hypervisor "sudo touch /etc/containers/enable-rosetta"
podman machine stop podmachine-apple-hypervisor
podman machine start podmachine-apple-hypervisor
```

Then we can start up the SQL Server container through the docker compose

No need to specify platform eventually like this