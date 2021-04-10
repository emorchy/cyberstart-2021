# fe04
 **100 points**

The moment I saw the description mention certain characters in groups, I knew that this was a regex challenge. For those who do not know what regex is or how to use it, I recommend [regexone](https://regexone.com).

| Challenge Description | Regex Translation|
| --- | --- |
|The username you are looking for has x as the 3rd character| ^..x|
|followed immediately by a number from 2 to 6|^..x[2-6]|
|It has a Z character in it| I do not know regex for this|
|The last character is S.|^..x[2-6].\*S$"|

We can grep for "Z" at the end because I do not know regex for this.

```
$ cat 50k-users.txt|grep -P "^..x[2-6].*S$"|grep Z
YXx52hsi3ZQ5b9rS
```
