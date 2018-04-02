+++
date = "2015-12-03T19:28:54-07:00"
title = "Enabling iTerm's `Cmd+K` hotkey for Cygwin shell with ConEmu"
+++

## Enabling iTerm's `Cmd+K` hotkey for Cygwin shell with ConEmu

Sometimes I have to use the shell (a command-line interface) on Windows and that sucks.

Since Windows ecosystem (including shells, `Powershell` and `cmd`) has been historically quite removed from POSIX/UNIX patterns we came to know and love, even performing basic tasks gets challenging for me.  

Fornunatelly, there is [Cygwin](en.wikipedia.org/wiki/Cygwin), an open-source project that wraps the Windows system calls and makes an imitation of a bash shell we came to know and love. It is pretty decent for most every day uses.

A decent terminal emulator application helps too. I use and recommend [ConEmu](https://github.com/Maximus5/ConEmu).

One thing I missed from iTerm2 is to be able to clear the session screen completely with "Cmd+K" hotkey. Here is how to reproduce the same functionality for Windows/ConEmu/Cygwin.  

There is a key difference between Unix and Windows shells: in Unix, keeping track of the buffer content is the responsibility of the terminal emulator (iTerm2, for example), however in Windows it is a part of the shell's session. As a result, clearing the buffer can be done with a shell command, `cls` in their case.

Since Cygwin is still a Windows shell, clearing screen is a shell command as well, which is invoked with an escape code. Here it is: `echo -e '\0033\0143'`.

Here is how to set up the hotkey binding with Conemu to achive the Cmd+K behaviour of iTerm2:

<p style="text-align:center">
   <img width="500px" src="/conemu.png">
</p>
