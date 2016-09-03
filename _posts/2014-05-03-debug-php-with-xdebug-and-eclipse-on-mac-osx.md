---
layout: post
title: Debugging PHP with Eclipse and Xdebug
---

This article is part 3 of the serie Debug PHP with Xdebug and Eclipse on Mac OSX. In [part 1]({% post_url 2016-09-03-installing-xdebug-on-mac-osx %}) I covered how you can install Xdebug on Mac OSX. [Part 2]({% post_url 2016-09-03-setting-up-xdebug-for-eclipse %}) covered how you can setup Eclipse for debugging PHP scripts.

## Using breakpoints

One of the greatest features of debugging PHP with Eclipse and Xdebug is the use of breakpoints. Most PHP developers, including myself, start debugging PHP by adding a line somewhere in the code—like the one below—and run it.

```php
var_dump($var); die()
```

PHP will run the code up to that point, dump one or more variables and then halt. This can be pretty tedious when your code becomes more complicated.


With breakpoints this becomes a lot easier. By double-clicking on a line of code you can set a breakpoint—indicated by a little bullet next to the line number. You can set multiple breakpoints throughout your code.



With your breakpoints set you can start the debug session. The debugger will run through your code and pause on each of your breakpoints.



You can step through each breakpoint by clicking the green-arrow icon in iconbar of the Debug tab. Or you can press F8.


## Inspect variables

Now that you have set a breakpoint and the debugger paused on it, you can evaluate the values of variables. You can do that in the Variables tab. All variables that are available in the script are shown. When you select a variable its contents are displayed. In the screenshot below you see that the value of $_GET["name"] is “Jeroen”.



It is even possible to change the values of certain variables (you can’t change system variables like $_GET or $_POST). You can do this by double-clicking on the value and typing the new value. When finished press Enter. Now when you continue the script it will use the new value.

## Improved var_dump and error messages

Another neat feature of Xdebug is that it improves the output of the var_dump() function and error messages. When you call var_dump the output automatically has color highlighting and indenting. This greatly improves the readability as seen in the following screenshot.



For error messages Xdebug automatically adds a stack trace with details about execution time and memory usage, as you can see below.



More on Xdebug’s stack traces: [http://www.xdebug.org/docs/stack_trace](http://www.xdebug.org/docs/stack_trace)

## Troubleshooting

### Breakpoints not working

This could indicate that your path mapping isn’t set correctly. I once had this problem. Only the breakpoints in my index file were taken by the debugger. Breakpoints in other files that were included were ignored.

Using Xdebug’s remote log function I [managed to figure out](http://stackoverflow.com/questions/3422433/xdebug-ignores-breakpoints/4984177#4984177) that the paths Xdebug used were wrong. Be sure to check your path mappings when your breakpoints are being ignored.

### Waiting for XDebug session to start

When you start a debug session and Eclipse doesn’t finish because it’s waiting for a Xdebug session to start, you might have to check your firewall settings, or setup port forwarding on you router. Xdebug usually connects on port 9000 (see your php.ini settings). Eclipse listens on that port for incoming connections. If this port is blocked it won’t work.

### Errors not shown or only shown as text (not html)

If you don’t see any errors show up it might be possible that PHP’s setting display_errors is turned off. You can turn this on in your PHP scripts by inserting the following line somewhere at the top of you PHP script.

```php
ini_set("display_errors", 1);
```

Or you can enable it via you .htaccess with the following line.

```
php_flag display_errors on
```

If you only see text-only versions of the Xdebug var_dumps or stack traces, it could be that PHP’s setting html_errors is turned off. You can turn this on in your PHP scripts by inserting the following line somewhere at the top of you PHP script.

```php
ini_set("html_errors", 1);
```

Or you can enable it via you .htaccess with the following line.

```
php_flag html_errors on
```

This was the last part of the serie Debug PHP with Xdebug and Eclipse on Mac OSX. If you have any questions feel free to ask them in the comments below.

Happy debugging!
