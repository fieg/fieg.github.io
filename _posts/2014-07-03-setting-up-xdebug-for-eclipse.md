---
layout: post
title: Setting up Xdebug for Eclipse
---

This article is part 2 of the serie [Debug PHP with Xdebug and Eclipse on Mac OSX]({% post_url 2014-05-03-debug-php-with-xdebug-and-eclipse-on-mac-osx %}). In [part 1]({% post_url 2014-06-03-installing-xdebug-on-mac-osx %}) we covered how you can install Xdebug on Mac OSX.

Before we begin you will need a fully functional Eclipse PDT installation. If you don’t have it yet you can [download the PDT all-in-one package](http://www.eclipse.org/pdt/downloads/). After downloading you can extract the contents to a location of your liking. For example in /Applications/eclipse.

Ok, so we have Xdebug and Eclipse installed and can now start configuring Eclipse.


Start Eclipse and open preferences. Open the left-sided tree to PHP > PHP Servers



Select the Workspace Default server and click Edit.



Now add a new path. Set the Path on Server to the absolute path of your directory containing the files you want to debug. Set the Path in Workspace to the corresponding path in your workspace.

Next, open the left-sided tree to PHP > Debug.



Select XDebug as PHP Debugger, set the Server to the server you just configured and click Ok.

Next, open the PHP Debug perspective. From the Run menu choose Debug Configurations.

Doubleclick on PHP Web Page to create a new configuration. Select XDebug as Server Debugger—if that’s not already the case. In the File field choose the file you want to debug.

Deselect the Auto Generate checkbox in the URL field and set the correct path to your script on the server. Click on Apply and click on Debug.



You should see Eclipse launch your script as shown above and you should see a new browser session being started. From here on you can start debugging your scripts.

Once you’ve setup the Debugging Configuration for your script and did a first time debug, you can start the next debug session from the debug menu in the menubar.



For each new project that you want to debug you will have to create a new Debug Configuration.

Continue reading how to [Debug PHP with Eclipse and Xdebug]({% post_url 2014-05-03-debug-php-with-xdebug-and-eclipse-on-mac-osx %}).
