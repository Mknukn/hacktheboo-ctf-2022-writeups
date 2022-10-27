# Evaluation Deck

Checking the website:

![Untitled](Evaluation%20Deck%208a5eba61e1624f6c9dbf55456eea0eb7/Untitled.png)

When we try to play any cards here, it decreases the health of the ghost we are fighting. On the background, what’s actually happening is we are actually calling the `/api/get_health` endpoint:

![Untitled](Evaluation%20Deck%208a5eba61e1624f6c9dbf55456eea0eb7/Untitled%201.png)

We are passing in the `current_health` of the ghost, the `attack_power` if the card we flipped, and the `operator` of what operation we will do on this.

From the following code snippet, this is what the function is doing when we’re calling in the `/api/get_health` endpoint:

```python
def count():
    if not request.is_json:
        return response('Invalid JSON!'), 400

    data = request.get_json()

    current_health = data.get('current_health')
    attack_power = data.get('attack_power')
    operator = data.get('operator')
    
    if not current_health or not attack_power or not operator:
        return response('All fields are required!'), 400

    result = {}
    try:
        code = compile(f'result = {int(current_health)} {operator} {int(attack_power)}', '<string>', 'exec')
        exec(code, result)
        return response(result.get('result'))
    except:
        return response('Something Went Wrong!'), 500
```

From this code snippet, what we’re concerned about is this one:

```python
    result = {}

		try:
        code = compile(f'result = {int(current_health)} {operator} {int(attack_power)}', '<string>', 'exec')
        exec(code, result)
        return response(result.get('result'))
    except:
        return response('Something Went Wrong!'), 500
```

We are calling the `compile()` function which compiles the source code which is the first argument. The second argument is some recognizable value since the code was not read from a file (this is why `<string>` was used). The third argument specifies what kind of code must be compiled which is in our case is `exec` if our source consists of a ********************************************sequence of statements********************************************.

From here, what I initially did was do a recreation and play around with the result in my local machine:

![Untitled](Evaluation%20Deck%208a5eba61e1624f6c9dbf55456eea0eb7/Untitled%202.png)

From the screenshot above, we are generating a `code` object which has tons of other information. We also have the final `result` which is stored in the key `result`.

From the code snippet provided above, we can see that `attack_power` and `current_health` are being turned into integer types but we have free reign over the `operator` parameter.

We also have to remember that the `exec` function executes code as a sequence of statements. This means we can do command injection on the endpoint:

![Untitled](Evaluation%20Deck%208a5eba61e1624f6c9dbf55456eea0eb7/Untitled%203.png)

But this doesn’t help us since when we try to replicate this type of payload on the web app, we are getting back the result of our code which is `0` since `os.system` returns a `0` value when the code execution was done properly:

![Untitled](Evaluation%20Deck%208a5eba61e1624f6c9dbf55456eea0eb7/Untitled%204.png)

After a day of frustration, I cleared my head. Since the web app stores our result as a value in the `result` key, why not just add another value in there?

In Python, we can actually store multiple variables making our variable into a tuple:

```python
a = 10, 50

print(a) # returns (10, 50)
print(type(a)) # returns <class 'tuple'>
```

But how do we deal with the rest of the code after our injection? Simple. Just add a `#` to comment it out.

Our finally payload will now be:

```python
, open('../flag.txt' , 'r').read()#
```

Testing it out:

![Untitled](Evaluation%20Deck%208a5eba61e1624f6c9dbf55456eea0eb7/Untitled%205.png)

Our flag:

**HTB{c0d3_1nj3ct10ns_4r3_Gr3at!!}**