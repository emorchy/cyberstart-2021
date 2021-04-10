# fm02
 **250 points**

At first I assumed the challenge incorporated OCSP, but I later noticed the IRC protocol and immediately realized that I would be sifting through logs.

![IRC](https://i.imgur.com/dRkaOqt.png)
![IRC_Stream](https://i.imgur.com/F9ib1MM.png)

"TWFyaW9SdWxlejE5ODU=" appeared in the irc stream, and base64 decoding it outputs: "MarioRulez1985", a password which will be used later.
There is a file sent through IRC, as seen through the line `.DCC SEND "Flag.7z" 3232247681 35289 3466`.

According to [Wikipedia](https://en.wikipedia.org/wiki/Direct_Client-to-Client#DCC_SEND):

```
The original handshake consisted of the sender sending the following CTCP to the receiver:
DCC SEND <filename> <ip> <port>
```

So, the file "Flag.7z" is sent to IP 192.168.47.129 ([decimal to ip](https://www.browserling.com/tools/dec-to-ip)) through port 35289.

![File](https://i.imgur.com/Lhl5caR.png)
![File_Stream](https://i.imgur.com/tLfKnEt.png)

Saving the sent file as hex and decoding it shows that it is a 7z file.
```
$ cat Flag.7z.hex|hex -d|file -
/dev/stdin: 7-zip archive data, version 0.4
```

Unarchiving the 7z archive requires a password, so we use the base64 decoded password "MarioRulez1985" and voila, Flag.nes appears.

Recognizing the file extension, the "nes" is the file extension for a Nintendo ROM. I used pihole to emulate the ROM because I felt the most comfortable with it, as I have used it in the past.

![NES1](https://i.imgur.com/SGDG8v6.png)
![NES2](https://i.imgur.com/F7wnFAs.png)

Funny enough, I was too wrapped up in the fun of forensic analysis to realize that running `strings` may output something. Sure enough:

```
$ strings Flag.nes
--snip--
`Princess isn't in this castle.
Did you just run strings?
You have found the flag!
but you found the flag!
Flag: NESted_in_a_PCAP
Well Done Squire!
--snip--
```

This was an extremely fun challenge, and I learned a lot about network exfiltration methods and the analysis of different protocols.
