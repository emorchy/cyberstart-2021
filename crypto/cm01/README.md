# cm01
 **250 points**

We are given two images, one being a qr code and another being a seemingly corrupted qr code.
Scanning the qr code yields:
```
$ zbarimg frame.png
QR-Code:Hey, I've put the flag into the other file using the same trick we always use.  You know what to do. :)
```

Putting the flag into the other file... that looks like XOR to me!

```
from PIL import Image, ImageChops
im1 = Image.open("frame.png").convert("1")
im2 = Image.open("code.png").convert("1")
im3 = ImageChops.logical_xor(im1, im2)
im3.save("xor.png")
```

Opening the file shows a different QR code. Let's read it!

```
zbarimg xor.png 
QR-Code:FLAG: A_Code_For_A_Code
```
