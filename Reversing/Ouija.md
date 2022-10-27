# Ouija

Opening the binary in GDB:

![image](https://user-images.githubusercontent.com/86394721/198369525-7859b3dc-dc94-437c-8bba-bede290e9f84.png)

What stood out to me was `<flag>` which was `38` instructions away from the start of `main()`.  Basically the address at which `rip+/-0xe9f` is pointing to will be moved to the `rdi` register.

Seeting up a breakpoint at `+38` and `+45` yields us the flag at `breakpoint +45` which means that `$rdi` is now populated:

![image](https://user-images.githubusercontent.com/86394721/198369529-1221f8c9-6ea1-401b-ac1d-b6861cbdc521.png)

To get the string value of the register, we will use the `x` command of GDB which displays the memory contents of a given address using the specified format. We’ll be getting it as a string so our command will be `x/s $rdi`:

![image](https://user-images.githubusercontent.com/86394721/198369531-caa52a36-4088-4283-9ef5-4a80de06b850.png)

This looks like it’s a Caesar cipher so we can bruteforce that online:

![image](https://user-images.githubusercontent.com/86394721/198369535-d835c84c-359f-4855-a896-5807b3a3b81e.png)

Our flag:

**HTB{Adding_sleeps_to_your_code_makes_it_easy_to_optimize_later!}**