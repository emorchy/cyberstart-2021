# fe03
 **100 points**

We are told that the gunzip file is a docker image, so let's load the image:

```
$ docker load < fe03.tar.gz
Loaded image: ctf/fe03:v1
```

Let's search for the docker image:

```
$ docker image ls
REPOSITORY                           TAG                 IMAGE ID            CREATED             SIZE
ctf/fe03                             v1                  498fd7e65401        3 weeks ago         12.5MB
```

Thankfully, we are able to spawn a shell in the docker image:
```
$ docker run -it 498fd7e65401 /bin/bash
bash-5.1# 
```

Now we must search for the flag:

```
bash-5.1# find /|grep flag
./home/secret/flag.zip
--snip--
```

Trying to unzip the archive was attempted, but it is password protected.

Upon further digging, there is a file called ".bash_history" in the root directory. This file is a log of every bash command run by the user.

```
bash-5.1# cat /root/.bash_history 
cd /home/secret
zip -P taskdeadlyauditorywoodwork flag.zip flag.txt
rm /home/secret/flag.txt
```

The password for the archive is "taskdeadlyauditorywoodwork". Unzipping:

```
bash-5.1# unzip -P taskdeadlyauditorywoodwork flag.zip flag.txt
Archive:  flag.zip
extracting: flag.txt
bash-5.1# cat flag.txt
Flag: 8191-SiMpLeFilESysTemForens1Cs
```
