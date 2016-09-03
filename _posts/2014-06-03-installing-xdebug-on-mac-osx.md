---
layout: post
title: Installing Xdebug on Mac OSX
---

This article is part 1 of the serie [Debug PHP with Xdebug and Eclipse on Mac OSX.]({% post_url 2014-05-03-debugging-php-with-eclipse-and-xdebug %})


Before we begin you will need the following requirements:

* Mac OSX Install DVD
* Default PHP installation on Mac OSX

## 1. Install Xcode from your Mac OSX Install DVD

Before we can start installing XDebug we need to install Xcode. It’s required by the Pecl installer (next step). You can find it on your Mac OSX Install DVD in the Optionals folder.

Just follow the instructions of the Xcode installer.

## 2. Install Xdebug using Pecl

Open Terminal. Type in the following command:

```sh
$ sudo pecl install xdebug
```

This will install xdebug for you. When it is successfully installed it should state something like: “install ok”.

## 3. Enable the Xdebug PHP extension

First we have to locate the XDebug extension. Run the following command in your terminal.

```sh
$ locate xdebug.so
```

In case multiple locations are retruned: mine was located in /usr/lib/php/extensions/no-debug-non-zts-20090626/xdebug.so, see if there is a similar location. Remember the path.

Now open your php.ini file with your favorite editor (vim in my case). Your php.ini is probably located in /private/etc/php.ini. If you can’t find it try running the following command in the terminal.

```sh
$ php -i | grep "php.ini"
```

This should tell you what the loaded php.ini is.

The following line should be inserted. I placed mine on the end of the “Dynamic Extensions” section of the php.ini file.

```sh
zend_extension=/usr/lib/php/extensions/no-debug-non-zts-20090626/xdebug.so
```

Of course, you have to replace the path to the extension with the path you remembered earlier.

Do not close your editor yet. We have some more lines to add:

```
[xdebug]
xdebug.remote_enable=1
xdebug.remote_host=localhost
xdebug.remote_port=9000
xdebug.remote_handler="dbgp"
```

You can place these lines somewhere at the end of the php.ini file. The exact place doesn’t really matter. Although you have to make sure that these lines aren’t already in the php.ini (might be set by the Pecl installer).

## 4. Restart apache

The final step is restarting the Apache process. Open System Preferences > Sharing. Deselect Web Sharing. When Web Sharing is Off, select it again.

Xdebug should now be working. You can verify this by running the following command in Terminal.

```sh
$ php -v
```

It should show you something like this:

```sh
PHP 5.3.3 (cli) (built: Aug 22 2010 19:41:55)
Copyright (c) 1997-2010 The PHP Group
Zend Engine v2.3.0, Copyright (c) 1998-2010 Zend Technologies
    with Xdebug v2.1.0, Copyright (c) 2002-2010, by Derick Rethans
```

Continue reading how to [Setup Xdebug for Eclipse.]({% post_url 2014-07-03-setting-up-xdebug-for-eclipse %})
