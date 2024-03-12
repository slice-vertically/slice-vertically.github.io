---
layout: post-with-tags
title:  "Inspect layers of Docker image"
date:   2024-03-12
tags:   docker
---

```bash
docker history library/ubuntu --no-trunc --format 'table'
```