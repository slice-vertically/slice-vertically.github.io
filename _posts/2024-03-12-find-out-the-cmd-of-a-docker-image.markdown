---
layout: post-with-tags
title:  "Find out the CMD of a Docker image"
date:   2024-03-12
tags:   docker
---

{% raw %}
```sh
docker inspect --format='{{Config.Cmd}}' ubuntu
```
{% endraw %}