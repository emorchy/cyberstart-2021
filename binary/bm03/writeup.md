# bm03
 **250 points**
```
000000000000078a <output>:
 78a:	55                   	push   rbp
 78b:	48 89 e5             	mov    rbp,rsp
 78e:	48 81 ec 30 08 00 00 	sub    rsp,0x830
 795:	89 bd dc f7 ff ff    	mov    DWORD PTR [rbp-0x824],edi
 79b:	89 b5 d8 f7 ff ff    	mov    DWORD PTR [rbp-0x828],esi
 7a1:	64 48 8b 04 25 28 00 	mov    rax,QWORD PTR fs:0x28
 7a8:	00 00 
 7aa:	48 89 45 f8          	mov    QWORD PTR [rbp-0x8],rax
 7ae:	31 c0                	xor    eax,eax
 7b0:	48 8d 85 f0 f7 ff ff 	lea    rax,[rbp-0x810]
 7b7:	48 8d 15 42 02 00 00 	lea    rdx,[rip+0x242]        # a00 <_IO_stdin_used+0x60>
 7be:	b9 ff 00 00 00       	mov    ecx,0xff
 7c3:	48 89 c7             	mov    rdi,rax
 7c6:	48 89 d6             	mov    rsi,rdx
 7c9:	f3 48 a5             	rep movs QWORD PTR es:[rdi],QWORD PTR ds:[rsi]
 7cc:	c6 45 ed 20          	mov    BYTE PTR [rbp-0x13],0x20
 7d0:	c6 45 ee 5f          	mov    BYTE PTR [rbp-0x12],0x5f
 7d4:	c6 45 ef 2f          	mov    BYTE PTR [rbp-0x11],0x2f
 7d8:	c6 45 f0 5c          	mov    BYTE PTR [rbp-0x10],0x5c
 7dc:	c6 45 f1 28          	mov    BYTE PTR [rbp-0xf],0x28
 7e0:	c6 45 f2 29          	mov    BYTE PTR [rbp-0xe],0x29
 7e4:	c6 45 f3 60          	mov    BYTE PTR [rbp-0xd],0x60
 7e8:	c6 45 f4 2c          	mov    BYTE PTR [rbp-0xc],0x2c
 7ec:	c6 45 f5 7c          	mov    BYTE PTR [rbp-0xb],0x7c
 7f0:	c6 45 f6 2e          	mov    BYTE PTR [rbp-0xa],0x2e
 7f4:	c6 45 f7 00          	mov    BYTE PTR [rbp-0x9],0x0
 7f8:	c7 85 e8 f7 ff ff 00 	mov    DWORD PTR [rbp-0x818],0x0
 7ff:	00 00 00 
 802:	e9 88 00 00 00       	jmp    88f <output+0x105>
 807:	c7 85 ec f7 ff ff 00 	mov    DWORD PTR [rbp-0x814],0x0
 80e:	00 00 00 
 811:	eb 5d                	jmp    870 <output+0xe6>
 813:	8b 85 ec f7 ff ff    	mov    eax,DWORD PTR [rbp-0x814]
 819:	48 63 c8             	movsxd rcx,eax
 81c:	8b 85 e8 f7 ff ff    	mov    eax,DWORD PTR [rbp-0x818]
 822:	48 63 d0             	movsxd rdx,eax
 825:	48 89 d0             	mov    rax,rdx
 828:	48 c1 e0 02          	shl    rax,0x2
 82c:	48 01 d0             	add    rax,rdx
 82f:	48 89 c2             	mov    rdx,rax
 832:	48 c1 e2 04          	shl    rdx,0x4
 836:	48 01 d0             	add    rax,rdx
 839:	48 01 c8             	add    rax,rcx
 83c:	8b 8c 85 f0 f7 ff ff 	mov    ecx,DWORD PTR [rbp+rax*4-0x810]
 843:	ba 1f 85 eb 51       	mov    edx,0x51eb851f
 848:	89 c8                	mov    eax,ecx
 84a:	f7 ea                	imul   edx
 84c:	c1 fa 05             	sar    edx,0x5
 84f:	89 c8                	mov    eax,ecx
 851:	c1 f8 1f             	sar    eax,0x1f
 854:	29 c2                	sub    edx,eax
 856:	89 d0                	mov    eax,edx
 858:	48 98                	cdqe   
 85a:	0f b6 44 05 ed       	movzx  eax,BYTE PTR [rbp+rax*1-0x13]
 85f:	0f be c0             	movsx  eax,al
 862:	89 c7                	mov    edi,eax
 864:	e8 c7 fd ff ff       	call   630 <putchar@plt>
 869:	83 85 ec f7 ff ff 01 	add    DWORD PTR [rbp-0x814],0x1
 870:	8b 85 ec f7 ff ff    	mov    eax,DWORD PTR [rbp-0x814]
 876:	3b 85 d8 f7 ff ff    	cmp    eax,DWORD PTR [rbp-0x828]
 87c:	7c 95                	jl     813 <output+0x89>
 87e:	bf 0a 00 00 00       	mov    edi,0xa
 883:	e8 a8 fd ff ff       	call   630 <putchar@plt>
 888:	83 85 e8 f7 ff ff 01 	add    DWORD PTR [rbp-0x818],0x1
 88f:	8b 85 e8 f7 ff ff    	mov    eax,DWORD PTR [rbp-0x818]
 895:	3b 85 dc f7 ff ff    	cmp    eax,DWORD PTR [rbp-0x824]
 89b:	0f 8c 66 ff ff ff    	jl     807 <output+0x7d>
 8a1:	83 bd dc f7 ff ff 05 	cmp    DWORD PTR [rbp-0x824],0x5
 8a8:	7f 0c                	jg     8b6 <output+0x12c>
 8aa:	48 8d 3d 0f 01 00 00 	lea    rdi,[rip+0x10f]        # 9c0 <_IO_stdin_used+0x20>
 8b1:	e8 8a fd ff ff       	call   640 <puts@plt>
 8b6:	90                   	nop
 8b7:	48 8b 45 f8          	mov    rax,QWORD PTR [rbp-0x8]
 8bb:	64 48 33 04 25 28 00 	xor    rax,QWORD PTR fs:0x28
 8c2:	00 00 
 8c4:	74 05                	je     8cb <output+0x141>
 8c6:	e8 85 fd ff ff       	call   650 <__stack_chk_fail@plt>
 8cb:	c9                   	leave  
 8cc:	c3                   	ret
```

The program seems to fail around the `jl     807 <output+0x7d>` because the outputted lines are halted at one point. Additionally, this is the only jump that points near the top of objdump to parse another line. Thus, changing the jl to an unconditional jump would print every line. There are several methods to patch the file, but I will only show one. To use GDB and Ghidra, look back to my writeup for bm02.

### Hexedit
By far the easiest is to edit the binary itself. Because the jl instruction is at 0x89b, we know where to look:
```
00000894   FF 3B 85 DC  F7 FF FF 0F  8C 66 FF FF  .;.......f..
000008A0   FF 83 BD DC  F7 FF FF 05  7F 0C 48 8D  ..........H.
```
The assembled instruction for an intel jumpq (jump to address) is 0x48e9 with the address (already provided).

```
00000894   FF 3B 85 DC  F7 FF FF 48  E9 66 FF FF  .;.....H.f..
000008A0   FF 83 BD DC  F7 FF FF 05  7F 0C 48 8D  ..........H.
```

![hexedit_patch](https://i.imgur.com/L2JXRfj.png)

Running it produces:
```
 Flag:
       __       __                          _                      ____ __           
  ____/ /___   / /_   __  __ ____ _ ____ _ (_)____   ____ _       / __// /_ _      __
 / __  // _ \ / __ \ / / / // __ `// __ `// // __ \ / __ `/      / /_ / __/| | /| / /
/ /_/ //  __// /_/ // /_/ // /_/ // /_/ // // / / // /_/ /      / __// /_  | |/ |/ / 
\__,_/ \___//_.___/ \__,_/ \__, / \__, //_//_/ /_/ \__, /______/_/   \__/  |__/|__/  
                          /____/ /____/           /____//_____/
```
flag: debugging_ftw
