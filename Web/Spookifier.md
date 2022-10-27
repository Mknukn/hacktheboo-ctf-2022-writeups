# Spookifier

Checking the website:

![image](https://user-images.githubusercontent.com/86394721/198368857-7095ea17-7326-444b-b416-7fb59a7a4fa6.png)


It seems to change our name into spookified fonts. Reading the source code, I noticed in `utils.py` that they were using `mako.template` which has an SSTI vulnerability. From this [link](https://podalirius.net/en/articles/python-context-free-payloads-in-mako-templates/), we can do a simple recon if we can do SSTI with this following payload: `${self.module.cache.util.os}`.

But first, we have to look for where our injection will appear.

Checking the source code, it seems that `font4` will be where our SSTI be reflected since this font also deals with special characters.

Trying the payload out:

![image](https://user-images.githubusercontent.com/86394721/198368864-844abde1-22f4-4620-83f5-ae528132e8b8.png)

We have successful SSTI. After this, I just looked for a function within the `os` module that can print out our flag and I found `os.popen().read()`.

Our final payload will be:

```python
${self.module.cache.util.os.popen('cat ../flag.txt').read()} 

# We have to url encode our payload
%24%7Bself.module.cache.util.os.popen%28%27cat%20..%2Fflag.txt%27%29.read%28%29%7D
```

Testing it out:

![image](https://user-images.githubusercontent.com/86394721/198368868-a02c5a14-74a5-4481-9f94-fb4ca61019e5.png)

Our flag:

**HTB{t3mpl4t3_1nj3ct10n_1s_$p00ky!!}**
