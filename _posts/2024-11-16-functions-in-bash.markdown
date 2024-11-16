---
layout: post-with-tags
title:  "Functions in bash"
date:   2024-11-16
tags:   bash
---

```sh
foo()
{
  echo "hello from foo"
}

foo
# hello from foo
```

```sh
bar()
{
  echo "hello from bar, param is $1"
}

bar "baz"
# hello from bar, param is baz
```
