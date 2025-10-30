---
title: Pwninit Template
description: Template for pwninit
date: 2025-10-29 08:30:00 -0700
categories: [Script]
tags: []
---

```python
#!/usr/bin/env python3

from pwn import *
#import time
#from termcolor import colored
#from tqdm import tqdm

{bindings}
rop = ROP(elf)

context.binary = {bin_name}
context.terminal = ["alacritty", "-e", "sh", "-c"]
dbginit = """
b main
"""


def find_offset():
    r = process({proc_args})
    gdb.attach(r)
    p = cyclic(1000)
    r.sendline(p)
    r.interactive()


def conn():
    if args.REMOTE:
        r = remote("addr", 1337)
    elif args.GDB:
        r = gdb.debug({proc_args}, gdbscript=dbginit)
    else:
        r = process({proc_args})
    return r


def main():
    r = conn()

    sl  = lambda a   : r.sendline(a)
    sla = lambda a,b : r.sendlineafter(a,b)
    ru  = lambda a   : r.recvuntil(a)
    rud = lambda a   : r.recvuntil(a,drop=True)

    # r.interactive()
    

if __name__ == "__main__":
    main()
```
