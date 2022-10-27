# Ghost Wrangler

Opening the binary in GDB:

![image](https://user-images.githubusercontent.com/86394721/198369732-655b46c9-265e-46c8-bbb0-2285fdcd4b13.png)

What we’re most interested in is the `<get_flag>` function which is being called. Setting up a breakpoint around that area and a bit after that:

![image](https://user-images.githubusercontent.com/86394721/198369740-8a716e29-85de-4c19-aee5-171f6fd6bc18.png)

We see that the value of `$rax` and `$rdi` now holds our flag.

Our flag:

**HTB{h4unt3d_by_th3_gh0st5_0f_ctf5_p45t!}**