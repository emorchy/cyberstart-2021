# bm02
 **250 points**

Disassembly of main (objdump)
```
00000000000007e3 <main>:
 7e3:	55                   	push   rbp
 7e4:	48 89 e5             	mov    rbp,rsp
 7e7:	48 83 ec 10          	sub    rsp,0x10
 7eb:	c7 45 fc 01 00 00 00 	mov    DWORD PTR [rbp-0x4],0x1
 7f2:	81 7d fc 39 05 00 00 	cmp    DWORD PTR [rbp-0x4],0x539
 7f9:	75 0c                	jne    807 <main+0x24>
 7fb:	8b 45 fc             	mov    eax,DWORD PTR [rbp-0x4]
 7fe:	89 c7                	mov    edi,eax
 800:	e8 a5 fe ff ff       	call   6aa <printFlag>
 805:	eb 0c                	jmp    813 <main+0x30>
 807:	48 8d 3d 9a 00 00 00 	lea    rdi,[rip+0x9a]        # 8a8 <_IO_stdin_used+0x8>
 80e:	e8 5d fd ff ff       	call   570 <puts@plt>
 813:	b8 00 00 00 00       	mov    eax,0x0
 818:	c9                   	leave  
 819:	c3                   	ret    
 81a:	66 0f 1f 44 00 00    	nop    WORD PTR [rax+rax*1+0x0]
```

At 0x7f2, the cmp instruction does not pass because 0x1 is moved to the location at 0x7eb.
Thus, it is necessary to patch it. There are several methods to patch the file. I will show three.

### Hexedit
By far the easiest is to edit the binary itself. Because the mov instruction is at 0x7eb, we know where to look:
```
000007E4   48 89 E5 48  83 EC 10 C7  45 FC 01 00  00 00 81 7D  H..H....E......}
000007F4   FC 39 05 00  00 75 0C 8B  45 FC 89 C7  E8 A5 FE FF  .9...u..E.......
```
The value compared (0x539) appears in bytes 0x7F5 and 0x7F6, but the value moved (0x01) appears in bytes 0x7EE and 0x7EF. So edit the moved value to 0x539:

`000007E4   48 89 E5 48  83 EC 10 C7  45 FC 39 05  00 00 81 7D  H..H....E......}`

![hexedit_patch](https://i.imgur.com/DRmMOyg.png)

Save, and run. Output: `Flag: patchItFixIt`

### GDB
A second option would be to edit the value during the program's execution using GDB. I will be using pwndbg.
![setting](https://i.imgur.com/lzIVbgX.png)

Note the last two lines

![continue](https://i.imgur.com/ZThJvfL.png)

### Ghidra
Finally, Ghidra can be a simple tool to edit the instruction.

Patch the instruction by right clicking on `MOV        dword ptr [RBP + local_c],0x1` and selecting 'Patch Instruction'.

Then, edit the last value (0x1) and change to 0x539.

![patch](https://i.imgur.com/iYP4d0y.png)

In order to save the patch, I downloaded ghidra plugin "Save Patch", which can be downloaded (here)["https://github.com/schlafwandler/ghidra_SavePatch"].

Follow the instructions (don't forget to highlight the selected patch instruction, and run the script.
