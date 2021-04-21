# cm02
**250 points**

We are given a long file with many emoticons and punctuations. The spaces and punctuations seem to fit a standard structure, but the emoticons seem to substitute characters. We most likely have a substitution cipher.

First, let's see if it is possible to replace all the emojies with alpha characters.
Taking the first line:

```
test = '👶👉🍇👀 💪👨 👯🔥👯🏃👀👅💪🏃💪👀😰?'
[i.encode('utf-8') for i in test]
[b'\xf0\x9f\x91\xb6', b'\xf0\x9f\x91\x89', b'\xf0\x9f\x8d\x87', b'\xf0\x9f\x91\x80', b' ', b'\xf0\x9f\x92\xaa', b'\xf0\x9f\x91\xa8', b' ', b'\xf0\x9f\x91\xaf', b'\xf0\x9f\x94\xa5', b'\xf0\x9f\x91\xaf', b'\xf0\x9f\x8f\x83', b'\xf0\x9f\x91\x80', b'\xf0\x9f\x91\x85', b'\xf0\x9f\x92\xaa', b'\xf0\x9f\x8f\x83', b'\xf0\x9f\x92\xaa', b'\xf0\x9f\x91\x80', b'\xf0\x9f\x98\xb0', b'?']
```

So it seems that the emojies are UTF-8 encoded, which will help in the analysis. Additionally, they are not ascii characters:
`test[0].isascii()`, but the punctuation and spaces are: `test[4].isascii()`.

```
import string
import unidecode

with open('emojis.txt','r') as fd:
    test = fd.read().replace("\n"," ")

output = list(test)
used = []
alphabet = list(string.ascii_uppercase)

for num,char in enumerate(test):
    if char.isascii():
        output[num] = char
    elif char in used:
        output[num] = alphabet[used.index(char)]
    else:
        encoded = char.encode('utf-8')
        print(output[num])
        try:
            output[num] = alphabet[len(used)]
            used.append(char)
        except IndexError: #if all alphabet is exhaused
            output[num] = unidecode.unidecode(char) #deunicode (â = a, · = *, etc.)

final = ''.join(output)
with open('translated.txt','w') as fd:
    fd.write(final)
    fd.close()
```

Navigating to 'https://www.boxentriq.com/code-breaking/cryptogram' and pasting the output yields a large file, but most importantly, the flag hidden in the middle.

`flag: frequently_substitute_frowny_face_for_smiley_face`
