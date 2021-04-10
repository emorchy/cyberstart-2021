# fm02
 **250 points**

The name of the extracted file ("dir_volume") was unhelpful in my forensic analysis, and I gave up on this challenge and moved to other ones. Not much later, CyberStart released a hint to this challenge, as not one person has solved it yet. The hint was that the volume was an encrypted veracrypt drive. Bingo.

[Hashcat](https://hashcat.net/wiki/), an amazing password cracker, actually supports multiple veracrypt volumes:

```
$ hashcat --help
--snip--
  137XY | VeraCrypt                                        | Full-Disk Encryption (FDE)
     X  | 1 = PBKDF2-HMAC-RIPEMD160                        | Full-Disk Encryption (FDE)
     X  | 2 = PBKDF2-HMAC-SHA512                           | Full-Disk Encryption (FDE)
     X  | 3 = PBKDF2-HMAC-Whirlpool                        | Full-Disk Encryption (FDE)
     X  | 4 = PBKDF2-HMAC-RIPEMD160 + boot-mode            | Full-Disk Encryption (FDE)
     X  | 5 = PBKDF2-HMAC-SHA256                           | Full-Disk Encryption (FDE)
     X  | 6 = PBKDF2-HMAC-SHA256 + boot-mode               | Full-Disk Encryption (FDE)
     X  | 7 = PBKDF2-HMAC-Streebog-512                     | Full-Disk Encryption (FDE)
      Y | 1 = XTS  512 bit pure AES                        | Full-Disk Encryption (FDE)
      Y | 1 = XTS  512 bit pure Serpent                    | Full-Disk Encryption (FDE)
      Y | 1 = XTS  512 bit pure Twofish                    | Full-Disk Encryption (FDE)
      Y | 1 = XTS  512 bit pure Camellia                   | Full-Disk Encryption (FDE)
      Y | 1 = XTS  512 bit pure Kuznyechik                 | Full-Disk Encryption (FDE)
      Y | 2 = XTS 1024 bit pure AES                        | Full-Disk Encryption (FDE)
      Y | 2 = XTS 1024 bit pure Serpent                    | Full-Disk Encryption (FDE)
      Y | 2 = XTS 1024 bit pure Twofish                    | Full-Disk Encryption (FDE)
      Y | 2 = XTS 1024 bit pure Camellia                   | Full-Disk Encryption (FDE)
      Y | 2 = XTS 1024 bit pure Kuznyechik                 | Full-Disk Encryption (FDE)
      Y | 2 = XTS 1024 bit cascaded AES-Twofish            | Full-Disk Encryption (FDE)
      Y | 2 = XTS 1024 bit cascaded Camellia-Kuznyechik    | Full-Disk Encryption (FDE)
      Y | 2 = XTS 1024 bit cascaded Camellia-Serpent       | Full-Disk Encryption (FDE)
      Y | 2 = XTS 1024 bit cascaded Kuznyechik-AES         | Full-Disk Encryption (FDE)
      Y | 2 = XTS 1024 bit cascaded Kuznyechik-Twofish     | Full-Disk Encryption (FDE)
      Y | 2 = XTS 1024 bit cascaded Serpent-AES            | Full-Disk Encryption (FDE)
      Y | 2 = XTS 1024 bit cascaded Twofish-Serpent        | Full-Disk Encryption (FDE)
      Y | 3 = XTS 1536 bit all                             | Full-Disk Encryption (FDE)
--snip--
```

Those are a lot of options! Which one do we choose?

Thankfully, the "Y" option has a number "3" that can test all the Y options, preventing constant adjustments and headaches. I decided to go in order of the X values with a small password list.

```
$ hashcat -m 13723 --force dir_volume 1000-most-common-passwords.txt
--snip--
dir_volume:redwings                              
--snip--
```

After two tries, the hashing algorithm turned out to be PBKDF2-HMAC-SHA512, with the password "redwings". All that is left is to decrypt with veracrypt.

![Veracrypt](https://i.imgur.com/zZaqflT.png)

The volume contains a simple folder titled "Flag" and a file named "flag.txt", with the contents being: `Us3_5tr0ng_P@55w0Rds!`
