# Evaluation Deck

Checking the website:

![image](https://user-images.githubusercontent.com/86394721/198368558-e76ca561-0d22-4fea-87f5-468b561ecf4b.png)

When we try to play any cards here, it decreases the health of the ghost we are fighting. On the background, what’s actually happening is we are actually calling the `/api/get_health` endpoint:

![image](https://user-images.githubusercontent.com/86394721/198368569-8c0be456-5a90-4b5a-a628-5418b2a0a87b.png)

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

![image](https://user-images.githubusercontent.com/86394721/198368586-4533a671-2dd4-4944-afc7-a44c4bcac9ea.png)

From the screenshot above, we are generating a `code` object which has tons of other information. We also have the final `result` which is stored in the key `result`.

From the code snippet provided above, we can see that `attack_power` and `current_health` are being turned into integer types but we have free reign over the `operator` parameter.

We also have to remember that the `exec` function executes code as a sequence of statements. This means we can do command injection on the endpoint:

![image](https://user-images.githubusercontent.com/86394721/198368596-986c238b-cc10-4818-9767-b9a3fb621c7c.png)

But this doesn’t help us since when we try to replicate this type of payload on the web app, we are getting back the result of our code which is `0` since `os.system` returns a `0` value when the code execution was done properly:

![image](https://user-images.githubusercontent.com/86394721/198368608-c52e5a0e-461e-40c6-a82b-6d5d0a0a9e40.png)

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

![image](https://user-images.githubusercontent.com/86394721/198368617-acda2725-b683-401c-9337-520b940e85ad.png)

Our flag:

**HTB{c0d3_1nj3ct10ns_4r3_Gr3at!!}**