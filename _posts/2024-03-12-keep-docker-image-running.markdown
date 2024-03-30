---
layout: post-with-tags
title:  "Keep Docker image running"
date:   2024-03-12
tags:   docker
---

```bash
docker run -dt ubuntu
```

This will run the `CMD` specified in the image (in this case `/bin/bash`) and then **keep it up and running**.

> ⚠ Not all images will keep running, `curl` for example will execute its `CMD` and then finish. [You can overwrite an image's CMD]({% link _posts/2024-03-12-overwrite-a-docker-images-cmd.markdown %}).