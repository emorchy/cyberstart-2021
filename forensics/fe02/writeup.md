# fe02
 **100 points**

We are given the domain "cfta-fe02.allyourbases.co". We can't seem to look at a website in a browser, but we can search for any DNS entries.
Typing `nslookup -type=any cfta-fe02.allyourbases.co` in a terminal yields:
```
Server:		127.0.0.53
Address:	127.0.0.53#53

Non-authoritative answer:
Name:	cfta-fe02.allyourbases.co
Address: 13.226.253.34
Name:	cfta-fe02.allyourbases.co
Address: 13.226.253.65
Name:	cfta-fe02.allyourbases.co
Address: 13.226.253.36
Name:	cfta-fe02.allyourbases.co
Address: 13.226.253.4
cfta-fe02.allyourbases.co	text = "flag=unlimited_free_texts"
https://dns-lookup.com/cfta-fe02.allyourbases.co
flag=unlimited_free_texts in the txt record
```
