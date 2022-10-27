# Juggling Facts

Checking the website:

![Untitled](Juggling%20Facts%20d8258b0778774ae79dfa3b8ba6f9cd17/Untitled.png)

When doing recon, we are calling `/api/getfacts` when we’re clicking for facts related to the pumpkins:

![Untitled](Juggling%20Facts%20d8258b0778774ae79dfa3b8ba6f9cd17/Untitled%201.png)

From the name of the challenge itself, we will be dealing with Type Juggling which is a feature in PHP which lets there be no need to explicitly define the ****type**** of any variable you declare, PHP will automatically convert the data into a common, comparable type.

Meaning that for example this code snippet:

```php
$string1 = 12;
$string2 = '12';

if ($string1 == $string2) {
	echo "They are the same!!!"; # Will print out because of loose comparison
}
```

But there are also different types of comparisons for PHP which is loose type that depends on `==` and strict comparison which depends on `===`.

From the code snippet below, the `$_SERVER['REMOTE_ADDR']` might be intimidating but we don’t really have to bypass any of that. What we are actually interested in is the switch case.

```php
if ($jsondata['type'] === 'secrets' && $_SERVER['REMOTE_ADDR'] !== '127.0.0.1')
        {
            return $router->jsonify(['message' => 'Currently this type can be only accessed through localhost!']);
        }

        switch ($jsondata['type'])
        {
            case 'secrets':
                return $router->jsonify([
                    'facts' => $this->facts->get_facts('secrets')
                ]);

            case 'spooky':
                return $router->jsonify([
                    'facts' => $this->facts->get_facts('spooky')
                ]);
            
            case 'not_spooky':
                return $router->jsonify([
                    'facts' => $this->facts->get_facts('not_spooky')
                ]);
            
            default:
                return $router->jsonify([
                    'message' => 'Invalid type!'
                ]);
        }
```

According to the PHP docs, switch case does loose comparison and switch cases go from the first check up until the last. If there is no match, it will go to the `default`. 

Since `'secrets'` is in the up, we can actually do loose comparison matching here. We also have to keep in mind that what we’re passing is a `JSON` body which means we are limited to strings, integers, arrays, and booleans.

Luckily for us, we have this table that comes in handy for loose comparisons:

![Untitled](Juggling%20Facts%20d8258b0778774ae79dfa3b8ba6f9cd17/Untitled%202.png)

We can then make our switch case true by supplying in `true` to our `type` parameter.

Testing it out:

![Untitled](Juggling%20Facts%20d8258b0778774ae79dfa3b8ba6f9cd17/Untitled%203.png)

Our flag:

**HTB{sw1tch_stat3m3nts_4r3_vuln3r4bl3!!!}**