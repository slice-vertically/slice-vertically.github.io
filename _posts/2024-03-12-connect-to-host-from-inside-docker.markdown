---
layout: post-with-tags
title:  "Connect to host from inside Docker"
date:   2024-03-12
tags:   docker
---

Start a `netcat` server in your host machine:
```sh
nc -l 8000
```

Then run a `curl` from within a Docker container:
```sh
docker run curlimages/curl host.docker.internal:8000
```

Notice we specify `host.docker.internal` instead of `localhost`.

> ⚠ Linux users have to add a flag for this to work: `--add-host host.docker.internal:host-gateway`