# Juggling Facts

Checking the website:

![image](https://user-images.githubusercontent.com/86394721/198368738-cc4baddd-815a-4c50-bd5a-de1221c5eac9.png)

When doing recon, we are calling `/api/getfacts` when we’re clicking for facts related to the pumpkins:

![image](https://user-images.githubusercontent.com/86394721/198368747-533754ef-ab15-459c-9d2b-55be6b892db0.png)

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

![image](https://user-images.githubusercontent.com/86394721/198368751-196b7720-344e-462b-9bc7-d6e3d63d72cd.png)

We can then make our switch case true by supplying in `true` to our `type` parameter.

Testing it out:

![image](https://user-images.githubusercontent.com/86394721/198368756-2e33e0d0-b177-4766-841c-1ab11f4c4daa.png)

Our flag:

**HTB{sw1tch_stat3m3nts_4r3_vuln3r4bl3!!!}**