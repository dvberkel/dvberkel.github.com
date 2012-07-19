---
layout: post
title: "Command Line Wizardry"
date: 2012-07-19 10:36
comments: true
categories: 
- command line
- tools
---

The command line is a powerfull tool. It harnessess the power to
slice and dice data from and into the system, transforming it for the
better.

[The pragmatic programmer][1] even devotes a tip on the use of [the
command line][2]:

{% blockquote The Pragmatic Programmer http://pragmatictips.com/21 Tip 21 %}
Every woodworker needs a good, solid, reliable workbench, somewhere to
hold work pieces at a convenient height while he or she works
them. The workbench becomes the center of the wood shop, the craftsman
returning to it time and time again as a piece takes shape. 

For a programmer manipulating files of text, that workbench is the
command shell. From the shell prompt, you can invoke your full
repertoire of tools, using pipes to combine them in ways never dreamt
of by their original developers. From the shell, you can launch
applications, debuggers, browsers, editors, and utilities. You can
search for files, query the status of the system, and filter
output. And by programming the shell, you can build complex macro
commands for activities you perform often. 
{% endblockquote %}

I recently learned a new command which I find very usefull. The
command is `cut` and it allows to cut a line of text along a delimeter
and select a portion from it.

From the manpage for `cut`

{% blockquote SS64 http://ss64.com/bash/cut.html manpage for cut  %}
The cut utility cuts out selected portions of each line (as specified
by list) from each file and writes them to the standard output.  If no
file arguments are specified, or a file argument is a single dash
(`-'), cut reads from the standard input.  The items specified by list
can be in terms of column position or in terms of fields delimited by
a special character.  Column numbering starts from 1.
{% endblockquote %}

For example the following command kills all the running java programs
with one command.

{% codeblock lang:bash %}
ps -u dvberkel | grep java | cut -d ' ' -f 4 | kill -9
{% endcodeblock %}

[1]: http://pragprog.com/the-pragmatic-programmer/ "The Pragmatic Programmer: From Journeyman to Master"
[2]: http://pragmatictips.com/21 "Use the Power of Command Shells tip on pragmatictips.com"
