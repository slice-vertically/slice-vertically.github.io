---
layout: post-with-tags
title:  "Overwrite a Docker image's CMD"
date:   2024-03-12
tags:   docker
---

```sh
docker run -dt --entrypoint '/bin/sh' curlimages/curl
```