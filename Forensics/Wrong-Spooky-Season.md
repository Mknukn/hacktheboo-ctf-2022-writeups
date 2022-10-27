# Wrong Spooky Season

We were given a `capture.pcap` for our forensics investigation. Checking it on Wireshark:

![image](https://user-images.githubusercontent.com/86394721/198369881-e50bfafb-ff96-4e9f-9379-d566f04d5f41.png)

Looks like our attacker was able to upload a web shell that can execute code.

Looking for the TCPstream which relates to the reverse shell, we find it as `No. 466` which starts with a `SYN` flag set for TCP:

![image](https://user-images.githubusercontent.com/86394721/198369885-9cc5fa6f-521d-4424-b87b-9659c9cb4a31.png)

Following its TCP stream:

![image](https://user-images.githubusercontent.com/86394721/198369887-bc7dff05-efdc-4e2c-9cc7-e40d68366fae.png)

Looks like we have a reversed `base64` encoded string. Reversing it and decoding it:

![image](https://user-images.githubusercontent.com/86394721/198369888-98b5ff26-a6a7-412b-a529-c7abc659eb2e.png)

Our flag:

**HTB{j4v4_5pr1ng_just_b3c4m3_j4v4_sp00ky!!}**