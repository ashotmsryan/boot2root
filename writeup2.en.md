# writeup 2

When we turn on the vm while holding **shift** button, it stays on **grub** and we can turn on the linux in different way from **grub**

<img src="img/Pasted image 20240712011804.png">

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
