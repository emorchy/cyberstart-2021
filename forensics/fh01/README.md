# fh01
 **500 points**

There seems to be some sort of an SSH connection, and the data transmitted is fragmented and transmitted through UDP. There are also mini headers in each fragmentation in the UDP section that represent each part of the file. They need to be combied to successfully decode.
According to an analysis of the client's transmitted udp packets via "Follow UDP Stream", the client calls for five files in sequential order:
```
1.jpg
2.txt
3.eps
4.jpg
5.zip
```

[This video](https://www.youtube.com/watch?v=1gX_-DqGNxA) talks about "Wireshark Fragmented Packet Capturing", and it uses a H.G. Wells book as an example file to transmit over wireshark. After analyzing the fragmented data of "2.txt", the file is an ebook with the same title and author, so I know I am on the right track.

![2.txt](https://i.imgur.com/UvtIsp9.png)

It seems that the fragmented data is transmitted in a certain order, with the first UDP packet of the series containing the full fragmented data, whereas the IPv4 packet before and UDP packet after do not contain the full fragmented data.

### First attempt
At first, I tried extracting all the last fragmented udp files with the filter: ip.src == 192.168.47.129 && udp.port == 5555, copying the steps of isolating the fragmented data in the video.

![1.jpg](https://i.imgur.com/njUD08A.png)

Sure enough, the JFIF file header appears in the "Reassembled IPv4" part of the second UDP packet. However, the data does not begin until after "01 00 00 00 00 00 00 00 00 08 00 00 00 00 00 00". The next packet reveals a similar format, but starts with "02 00 00 00 00 00 00 00 00 08 00 00 00 00 00 00". I realize that it represents the sequence in which the fragmented packets are transmitted, and it resets with each file back to "01 00 00 00 00 00 00 00 00 08 00 00 00 00 00 00".

My first attempt included saving the entire UDP stream as a hex dump and deleting the next occurence of the next file start sequence. I used vim to search and delete every sequence that did not belong to the file I was attempting to exfiltrate by running `/0000  01 001` and deleting the results before and after each one (`dG` and `dgg`)

Next, I used `grep` to focus on the hex dumps, `sed` to remove the packet sequence headers and `tr` to remove the newlines and combine the hex dumps into one colossal hex dump.

`cat 1.jpg.txt|grep -E "0000  (.. 0[012]) 00 00 00 00 00 00" -A 128|sed 's/^0000.\*//'|sed 's/^\-\*//'|tr -s '\n' > 1.jpg.hexdump`

Last, I converted the hexdump in [Cyberchef](https://gchq.github.io/CyberChef/#recipe=From_Hexdump()) and saved the result.

![1.jpg](https://i.imgur.com/2DX0ZHl.jpg)
![4.jpg](https://i.imgur.com/vyr5LQp.jpg)

The images pretty much speak for themselves. There was an issue with my extremely unreliable hack that corrupted the jpg files, prevented "4.eps" file from compiling, and preventing the unarchiving of "5.zip". "2.txt" seemed to come out alright, but then again, a byte missing in a text file is pretty hard to notice. With an image offset that looks extremely weird and some files that refuse to load, I decided to sleep on it and come back at it again later.

### Second attempt

After mulling things over, I decided to use [tshark](https://tshark.dev/), a command line version of wireshark. As it turns out, tshark is better.
Getting familiar with the command arguments took a little while, but after reading man pages and looking at _several_ examples, I created my one-liiner:

`tshark -r fh01.pcapng -T fields -Y "udp.port == 5555 && ip.src == 192.168.47.129" -e data > tshark.hex`

|Argument|Description|
|--|--|
|-r fh01.pcapng|Reads "fh01.pcapng" as input|
|-T fields|Set the format of the output when viewing decoded packet data to "fields".|
|-Y "udp.port == 5555 && ip.src == 192.168.47.129"|Sets the display filter, just like wireshark|
|-e data|Adds a list of fields to display|

The ability to specify the extraction of packet "data" fields worked out almost perfectly, skipping directly to the sequence number and transmitted data. Additionally, each data stream was in hex and separated my a newline, giving me the opportunity to skip the dodgy conversion from hexdump to hex. I manually split each transmitted file based on a small 6-8 byte data hexdump that separated each data stream, similar to the first attempt but more reliable.

I say the data extraction worked almost perfectly because there were some packets that were misinterpreted my wireshark and tshark as a packet different than a simple "UDP" protocol, removing the data fields from the packet. As a result, I manually add each data stream from the missing packets.

![Non-UDP-pure](https://i.imgur.com/WqK5cXR.png)

After some time, I noticed a pattern between the packets without a data field, like one packet appears every 255 packets at the 4th sequence. Additionally, there are around 8 packets that appear in one group, making manual adding easier. I added each stream by right clicking on the field that would have been the "data" field, and copying as a hex stream. It is easy to recognize which field contains the data because of the anticipated sequence number.

I also double checked to make sure every line contained an equal amount of characters. If I forgot to manually add a packet or incorrectly copied it, the command `while read line; do echo -n "$line" | wc -c; done < 1.jpg.txt` would show a value other than 4128 (2072 bytes times 2, b/c hex).

Finally, I ran `cat 1.jpg.txt|sed 's/^................................//g'|tr -d '\n' > 1.jpg.hexdump`, with different file names depending on the file I am currently extracting.

With fingers crossed, I converted the hex to raw binary with `hex -d 1.jpg.txt` and opened the file:

![1.jpg-2](https://i.imgur.com/f78Coxi.jpg)

Huzzah! It's normal! It was a pain to manually change the unique packets, but now we know that it works.

I was halfway through the ebook, before realizing that it would take a long time to manually edit each file, with the longest being 3.eps. So, I worked towards number 4, hoping to find the flag in the image:

![4.jpg-2](https://i.imgur.com/NYrxu5E.jpg)

Looks beautiful and uncorrupted! Although I didn't see a flag, it showed me that everything was going well and to keep pushing. Thank you, Commodore 64.

The next one that I attempted was the zip file. I thought, "If any file was going to contain a flag, it had to be the zip file. Also, it isn't that big of a file". Eventually, I could open the zip file and extract an image labeled 5.jpg.

![5.jpg](https://i.imgur.com/yYhQCAZ.jpg)

After working several hours on this challenge, I am proud to say that I learned how to exfiltrate fragmented data and learned some valuable tshark commands. Unfortunately, I still cannot figure out how to skip the manual adding of packets that do not contain a data field. If the flag was hidden in "3.eps", I would have had to find an automatic method to compensate for the odd packets.
