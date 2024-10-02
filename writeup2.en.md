# writeup 2

We have to do in the same way as in **writeup1** until using **curl** tool))

After doing that we have an incredible way to get the control of the root

**BACKDOOR**

<img src="img/photo_5435923932709510476_y.jpg">

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
