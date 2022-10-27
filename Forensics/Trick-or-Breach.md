# Trick or Breach

Opening our `capture.pcap` file in Wireshark:

![image](https://user-images.githubusercontent.com/86394721/198369222-edb25af5-32d4-4e46-a77f-55636e366661.png)

Looks like we have a lot of hex strings appended to the `.pumpkincorp.com` which can suggest that this might be some C2 traffic.

I used `zeek` and some bash magic in my method to exifltrate all the hexadecimals.

First we generate a list of logs from our `pcap` file:

![image](https://user-images.githubusercontent.com/86394721/198369236-bf079f49-ff63-40e2-87a4-c58701cc711b.png)

Then we use `zeek-cut` to get the hexadecimals:

![image](https://user-images.githubusercontent.com/86394721/198369240-854497ba-32ad-4672-ab28-8ac1e1ea7ba9.png)

Pasting all of this into Cyberchef:

![image](https://user-images.githubusercontent.com/86394721/198369243-4be583b3-1a97-4f22-90cd-0ebbe44e3c79.png)

Looks like it detected that the hexadecimals we exifltrated was actually a ZIP file. Saving this output into a file:

![image](https://user-images.githubusercontent.com/86394721/198369246-3573c119-9787-49ce-a953-e5ab89db0112.png)

Unzipping it, I did a quick `grep -iR` for recursive grep of our flag and we have:

![image](https://user-images.githubusercontent.com/86394721/198369249-d2fe302c-dbc7-469c-bb1a-0b89c2288201.png)

Our flag:

**HTB{M4g1c_c4nn0t_pr3v3nt_d4t4_br34ch}**