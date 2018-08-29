# Week-2

This week will be a continuation of week 1

## What to Read

One often used way to pass parameters to a service is through environment variable. As most of you will not have worked with those on Linux, I suggest you take a look at them. This text is long, but relatively easy to work through, and explains the important concepts as you go along.

* [How To Read and Set Environmental and Shell Variables on a Linux VPS](https://www.digitalocean.com/community/tutorials/how-to-read-and-set-environmental-and-shell-variables-on-a-linux-vps)

We will use environment variables for setting where the logs will located. And we will use the login scripts in the exercise below.

## Learning Goals

After this week you will be able to:
* Explain the difference between prevention, detection and recovery for systems you develop.
* Set up a firewall on ubuntu and examine the log files.
* Set up a remote logging server, and use that to register logins to an ubuntu server.
* **Plan how to use a cloud based logging service to enable anomaly detection.**

## Exercises

If your machine is hacked, you will not be able to look at the log files to see what happened. So it is good practice to log to a different machine. There are companies that offer this service, but we will try to set up a minimal system to do this ourselves to understand the principles.
There is a minimal logger system at:
https://github.com/DatSecurityCourseSpring2018/week1Logger
You need to compile it, copy the jar file to a droplet, and run the jar file (you will be asked to install some Java).
In addition, the environment variable `LOG_PATH` must be set for the program to work.

We will next make a different droplet where we will see if anyone logs in. I call this system “HoneyPot”.
The idea is that we will make  a user on HoneyPot, with a username of `test`, and password: `123456` (see https://en.wikipedia.org/wiki/List_of_the_most_common_passwords).
The login script of that user should log that a user logged in.
* Can you find out which IP that user came from?
* Which other info do we have we could log?

To log it - there is the “curl” command on linux. Try to figure out what this command does:
```
curl -G  "http://loggingsystem:8888/logger" --data-urlencode "log=Someone knocked"
```
What does `curl` do in the first place
What is the `-G` option, and what is the strange option at the end
Can we omit any of the flags?


Micro cheatsheet for making a new user on ubunto for our purpose:
```
useradd -m james
passwd james
```

You have to allow ssh-connection via password:

Look in `/etc/ssh/sshd_config` for
```
# Global settings
…
PasswordAuthentication no
…
```
Change to `yes`, then tell the sshd service to reload its configuration:
```
service ssh reload
```
To terminate all processes you might have started under the user johndoe:
```
pkill -u johndoe
```
That stops all processes - also johndoe’s interactive bash
