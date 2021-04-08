# bh01
 **500 points**

The program acts for a "magic word" and prints a strange value in return.
```
$ ./program
What is the magic word?
please
�:�lO?d#����*���E	
Did you understand that?
```

Odd enough, a different input returns the same output.

```
$ ./program
What is the magic word?
random
�:�lO?d#����*���E	
Did you understand that?
```

There is no function for the program labeled "main", however the main code resides in the text section of the elf function and is called from there. The first part of the text is unnecessary, as that is not the main function, rather it is more towards the middle. Footnotes throughout the code are explained at the bottom:

```
Disassembly of section .text:

0000000000001120 <.text>:
    --snip--
    Before main "function"
    --snip--
    1209:       f3 0f 1e fa             endbr64
    120d:       55                      push   rbp
    120e:       48 89 e5                mov    rbp,rsp
    1211:       48 81 ec 90 00 00 00    sub    rsp,0x90
    1218:       64 48 8b 04 25 28 00    mov    rax,QWORD PTR fs:0x28
    121f:       00 00 
    1221:       48 89 45 f8             mov    QWORD PTR [rbp-0x8],rax
    1225:       31 c0                   xor    eax,eax
    1227:       48 8d 45 80             lea    rax,[rbp-0x80]
    122b:       48 89 c7                mov    rdi,rax
    122e:       e8 bd fe ff ff          call   10f0 <time@plt>          [1]
    1233:       89 c7                   mov    edi,eax
    1235:       e8 96 fe ff ff          call   10d0 <srand@plt>         [2]
    --snip--
    Irrelevant code
    --snip--
    127a:       e8 31 fe ff ff          call   10b0 <puts@plt>          [3]
    127f:       48 8b 05 8a 2d 00 00    mov    rax,QWORD PTR [rip+0x2d8a]        # 4010 <stdout@@GLIBC_2.2.5>
    1286:       48 89 c7                mov    rdi,rax
    1289:       e8 72 fe ff ff          call   1100 <fflush@plt>
    128e:       48 8b 15 8b 2d 00 00    mov    rdx,QWORD PTR [rip+0x2d8b]        # 4020 <stdin@@GLIBC_2.2.5>
    1295:       48 8d 45 c0             lea    rax,[rbp-0x40]
    1299:       be 28 00 00 00          mov    esi,0x28
    129e:       48 89 c7                mov    rdi,rax
    12a1:       e8 3a fe ff ff          call   10e0 <fgets@plt>         [4]
    --snip--
    Obfuscation code
    --snip--
    160c:       8b 85 7c ff ff ff       mov    eax,DWORD PTR [rbp-0x84]
    1612:       39 85 78 ff ff ff       cmp    DWORD PTR [rbp-0x88],eax
    1618:       0f 82 1f fd ff ff       jb     133d <rand@plt+0x22d>    [5]
    161e:       eb 01                   jmp    1621 <rand@plt+0x511>
    1620:       90                      nop
    1621:       48 8d 45 90             lea    rax,[rbp-0x70]
    1625:       48 89 c7                mov    rdi,rax
    1628:       e8 83 fa ff ff          call   10b0 <puts@plt>          [6]
    162d:       48 8d 3d e8 09 00 00    lea    rdi,[rip+0x9e8]        # 201c <rand@plt+0xf0c>
    1634:       e8 77 fa ff ff          call   10b0 <puts@plt>          [7]
    1639:       b8 00 00 00 00          mov    eax,0x0
    163e:       48 8b 75 f8             mov    rsi,QWORD PTR [rbp-0x8]
    1642:       64 48 33 34 25 28 00    xor    rsi,QWORD PTR fs:0x28
    1649:       00 00 
    164b:       74 05                   je     1652 <rand@plt+0x542>
    164d:       e8 6e fa ff ff          call   10c0 <__stack_chk_fail@plt>
    1652:       c9                      leave  
    1653:       c3                      ret
```
[1] Gets the current time
[2] Receives pseudo random numbers with the time as the seed
[3] Prints "What is the magic word?"
[4] Receives input
[5] Jumps back to another iteration of obfuscation code if condition is met
[6] Prints output
[7] Prints "Did you understand that?"


After further testing, the program returns both `�:�lO?d#����*���E` and `Flag: aLitt���*���E`. It seems we are so close to getting the flag, so what could be wrong?

As it turns out, repeating the same input could sometimes yield two different outputs, and by analyzing the code, the only possible reason could be the time seed producing numbers that eventually lead to the output.

In order to find the correct time seed to produce the "Flag:.." output, I created a bash script:

```
$ for i in {1..60};
do
    sleep 1 #time updates every second
    ./program < word.txt #run program with word "please" in word.txt
    date +%s; #print the date format in hex
done

--snip--
What is the magic word?
�:�lO?d#����*���E	
Did you understand that?
1617857258
What is the magic word?
Flag: aLitt���*���E	
Did you understand that?
1617857259
--snip--
```

Half of the flag appears when the input is "please" and the time is 1617857259 seconds. To keep this permanent, overwrite the call to the time function with the seconds to pass it as the seed to srand().

![patch_time](https://i.imgur.com/oE2Aaib.png)

```
./patch_time
What is the magic word?
please
Flag: aLitt���*���E	
Did you understand that?
```

Now, no matter what time, the program will always print `Flag: aLitt���*���E`:

In order to print the full flag, the jb instruction at footnote [5] must always print to the source:
```
    160c:       8b 85 7c ff ff ff       mov    eax,DWORD PTR [rbp-0x84]
    1612:       39 85 78 ff ff ff       cmp    DWORD PTR [rbp-0x88],eax
    1618:       0f 82 1f fd ff ff       jb     133d <rand@plt+0x22d>
```
Editing 160c to move a larger value, say 0x30, to eax would meet the jump condition 50 times, therefore 50 iterations.

```
$ ./patch_full
What is the magic word?
please
Flag: aLittLeObfuScatIonalCharActEr
```
