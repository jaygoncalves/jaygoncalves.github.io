---
title: Over The Wire - Bandit
author: muqduq
date: 2020-08-25
categories: [Blogging, Learning, CTF]
tags: [linux, ctf, learning, overthewire, bandit, ssh]
---

## The Bandit's Journey

* * *

Overthewire (OTW) has a fantastic beginner style CTF that teaches you the basics of Linux.  I've decided to take the plunge and completely use Linux as my primary operating system. I've used it as a virtual machine in the past for sure, but I felt I was ready to commit. The command line is my new best friend, and OTW has made me fall deeper in love with it. Without further ado, here are my writeups for OTW - Bandit.    

### SPOILER ALERT 

*Here on out in this blog post there will be some spoiler alerts regarding flags.*

## BANDIT 0

This first level is pretty easy, we're going to use the SSH command to log in to the otw server.
The host is `bandit.labs.overthewire.org` and the port is `2220` 
The username we're using is `bandit0` and the password is `bandit0`

So, what is SSH? SSH stands for Secure Shell. SSH is a cryptographic network protocol that allows us to securely operate network services over an unsecured network. 

Connecting via SSH is quite simple. All you need to do is initiate the command in your command line with `ssh` then the username and the remote host.  You can also specify which port.  

`ssh <username>@<remote.host> -p 2220`

```
jason@admin-workstation:~/Desktop/CTF/OTW_BANDIT$ ssh bandit0@bandit.labs.overthewire.org -p 2220
This is a OverTheWire game server. More information on http://www.overthewire.org/wargames

```

From here you'll see some nice ASCII art and with some information regarding OTW Bandit and your next challenge.

Onto the next level!

## BANDIT 0 -> BANDIT 1

Here we have another simple challenge, we're learning to navigate our directories. We need to find a password hidden in a file called `readme`

I started off using the `ls` command to list the files in the directory. 

```
bandit0@bandit:~$ ls
readme

```
From here, I used the `file` command to see what type of file we're dealing with here.

```
bandit0@bandit:~$ file readme 
readme: ASCII text

```

So, we can see here that it's an ASCII Text file. Next, we're going to read the contents of this file with the `cat` command. *SPOILER ALERT AHEAD*

**Jason's Pro Tip: From here, I'll add the password to a text file in my CTF directory for future reference.**

```
bandit0@bandit:~$ cat readme 
boJ9jbbUNNfktd78OOpsqOltutMc3MY1

```

Now that we have the password we're going to log in to our next challenge. The user will be `bandit1` at the same host and port we used previously. 

## BANDIT 1 -> BANDIT 2

Okay, so we're in. Now we need to find the password stored in a file called `-` ... this is a wierd file name, okay let's go.

so same thing as last time, I performed the `ls` command to see what files are in the directory and I see the `-` file. I typed the `file -` command again like last time. But, this time my terminal wasn't acting properly. No file details were given, the terminal just returned a new line.

So, I used my Google-foo and researched this issue. I came across a helpful discussion on the Unix StackExhange forum.  From what I gathered, using `-` as an argument refers to **STDIN/STDOUT**. So, if we intend to open a file with a name similar to this we need to specify the full location: `./-` 

*SPOILER ALERT AHEAD*


```
bandit1@bandit:~$ cat ./-
CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9

```

## BANDIT 2 -> BANDIT 3

Same as last time, we're going to SSH into `bandit2` with the same host name and port.  Use the newly aquired password to get access.

This challenge wants us to find the password in a file called `spaces in this filename`. Seems simple enough, lets go!

So, just like last time I'm going to `ls` to list my files, and i see the target file. I use the `file` command with the trick we learned in our last challenge. But wait, look at this!  It just seems to list every word in the filename as an idivudal file.  Hmm... 

```
bandit2@bandit:~$ ls
spaces in this filename
bandit2@bandit:~$ file ./spaces in this filename
./spaces: cannot open `./spaces' (No such file or directory)
in:       cannot open `in' (No such file or directory)
this:     cannot open `this' (No such file or directory)
filename: cannot open `filename' (No such file or directory)
bandit2@bandit:~$ 

```

So, this is a simple fix.  We're going to just enclose the file name in single quotes. Alternativly, you can also do it this way: `cat spaces\ in\ this\ filename`


* * *

## References
https://en.wikipedia.org/wiki/Secure_Shell

https://unix.stackexchange.com/questions/16357/usage-of-dash-in-place-of-a-filename

