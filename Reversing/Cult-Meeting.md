# Cult Meeting

When we run the binary, we are greeted with:

![image](https://user-images.githubusercontent.com/86394721/198369430-b34c0dfe-aa48-4a92-b334-e9734f04e35c.png)

Let’s use `ltrace` to see what this is doing internally:

![image](https://user-images.githubusercontent.com/86394721/198369419-842f415b-fc5a-449e-8c30-856834539f1f.png)

Looks like it does a `strcmp()` with our input and `sup3r_s3cr3t_p455w0rd_f0r_u!`.

Trying it out on the docker container:

![image](https://user-images.githubusercontent.com/86394721/198369427-3125fb52-8acb-45e1-903c-dbbb4f4f3d8b.png)

Our flag:

************************HTB{1nf1ltr4t1ng_4_cul7_0f_str1ng5}************************

