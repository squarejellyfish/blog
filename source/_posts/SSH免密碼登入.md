---
title: SSH免密碼登入
date: 2024-09-14 21:09:44
tags:
---

1. ssh-keygen

```shell
$ ssh-keygen
```

會問你要把keygen存在哪，預設 `~/.ssh/`，然後問你要不要passphrase，直接enter就好

![](/images/1.png)

2. ssh-copy-id
```shell
$ ssh-copy-id username@server-ip
```

就這樣，可以免密碼登入了！
