+++
date = "2015-12-03T19:28:54-07:00"
title = "Enabling cls/Cmd+K for Cygwin Bash on ConEmu"
+++

Sometimes I have to use a shell (a command-line interface) on Windows and overall it is pretty challenging for me.
Windows is fundamentally different from the Unix/POSIX conventions I got used to when working with Linux and MacOSX and the native shells  for Windows (cmd/Powershell) are quite different.  

Fornunatelly, there is Cygwin, an open-source project that wraps the Windows functioanllity and makes some imitation of a bash we came to know and love that offers something similar to [bash](en.wikipedia.org/wiki/Cygwin). It is pretty decent for most every day uses and I like it a lot.

One thing that helps is a decent Terminal application (the thing that provides GUI for the cli session). [ConEmu](https://github.com/Maximus5/ConEmu) is pretty great, highly customizable and has all the basic features.

One thing I missed from iTerm2 is to be able to clear the session screen completely with "Cmd+K" letters. I got investigating and I am happy to report that I found the solution.  

It turns out that there is another key difference between Unix and Windows shells: in Unix, keeping track of printed content is the responsibility of the terminal application, but for Windows it is a part of the shell's session. As a result, clearing the screen can be done with a shell command, `cls` in their case.


So since Cygwin is still a Windows shell, clearing screen is a shell command as well, which is invoked with an escape code. Here it is: `echo -e '\0033\0143'`.

Here is how to set it up:

![Img](/conemu.png)

Let's set up ConEmu's hot key to use the command. Open up the ConEmu settings (right click on the button)
