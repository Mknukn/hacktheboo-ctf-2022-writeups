# Halloween Invitation

Checking our file:

![image](https://user-images.githubusercontent.com/86394721/198456726-e8b7a174-e702-4ba9-a6c9-7a0d0c7a58d1.png)

Looks like a Microsoft Word file. A `docm` file is a word file that has macros in it so we are probably interested with reverse engineering the macro for this document.

I did some Google search to find a way to extract our macros without running it on Windows and I stumbled upon this article: [Tools to extract VBA Macro source code from MS Office Documents | Decalage](https://www.decalage.info/vba_tools). 

I used `officerparser.py` to extract the macros and then we’ll look for the `ThisDocument.cls` file:

![image](https://user-images.githubusercontent.com/86394721/198456677-408f6749-1948-471e-92db-8d2f9e07f7b3.png)

What interested me most was `fxnrfzsdxmcvranp` which had lots of hex strings added to it. So what I did was get all the strings in double quotes:

![image](https://user-images.githubusercontent.com/86394721/198456695-2a1821b6-04f4-4ff0-958d-a37da4203360.png)

What we’re most interested in is for line `37-91`. After copying this to a new file, I had the problem that I needed to add each string that is from the same line so I created an easy python script for that:

```python
with open('numbers.txt' , 'r') as f:
    
    test = [i.strip("\n") for i in f.readlines()]
    home = []

    for num in range(0, len(test),2):
        newnum = test[num] + test[num+1]    
        home.append(newnum)

    
    for i in home:
        print(i)
```

After this, I now have this kind of output:

![image](https://user-images.githubusercontent.com/86394721/198456705-01df7231-3a97-4995-ba09-8767e3c67b46.png)

I had a bit of a hard time decoding this with Cyberchef so I had to decode it one by one. After finally decoding all 55 lines, I have my working file:

![image](https://user-images.githubusercontent.com/86394721/198456712-d27b9433-dcaa-4bc0-b454-8b20b6888710.png)

Now I just have to get all the `base64` encoded strings, join them together, and then decode it.

Decoding the payload:

![image](https://user-images.githubusercontent.com/86394721/198456719-574e590f-33f0-44d2-a394-7ae489d223bb.png)

Our flag:

**HTB{5up3r_345y_m4cr05}**