---
layout: post
title: "Introduction to the Command Line Interface (CLI)"
date: 2015-08-30 11:33:40
categories: technology life
comments: true
---

Basic `Command Line Interface` commands 

{% highlight bash %}
$ echo "Hello World" >> HelloWorld.txt
$ cat HelloWorld.txt | say
{% endhighlight %}

<br />

##The Command Line Interface

The Command Line Interface (CLI) is a human-computer interface that relies on text inputs and output.

![The terminal]({{ site.url }}/assets/introduction-to-command-line-interface/terminal.png)

<sub><sup>The Command Line Interface on a Macbook</sup></sub>

`This means you can type in commands and your computer will execute them!`

<br />

### The Unix philosophy

> Write programs that do one thing and do it well. 

The Command Line Interface (CLI) follows this philophy. The commands you use will be very self-contained and executes on its purpose in the most effective manner.

<br />

### Opening the CLI

You can access your Terminal on a `Macintosh` by:
	
- Pressing `Command + Spacebar`
- Type in `terminal`
- Press `<enter>`

<br />

### Basic commands

|Command|Meaning|
|---|---|
|pwd|print working directory|
|cd|change directory|
|ls|list|
|touch|creates a file|
|rm|remove|
|mkdir|make directory|
|rmdir|remove directory|
|mv|move|
|man|user manual|

<br />

### Getting started

In your terminal now, type the following:

**Note**: *The **$** stands for the beginning of input in the terminal. You do not type the $*

{% highlight bash %}
$ cd ~
$ pwd   
$ mkdir TheAwesomeTutorial
$ cd TheAwesomeTutorial
$ touch stanley is cool
$ ls -l
$ mv * ..
$ cd ..
$ ls
$ rm -rf TheAwesomeTutorial stanley is cool
{% endhighlight %}

### The results

![The steps]({{ site.url }}/assets/introduction-to-command-line-interface/tutorial.png)

<sub><sup>What it looks like in my terminal</sup></sub>

<br />

### Understanding what happened

1. We changed to the home directory, in my case it's `/Users/stanley`. The `~` denotes `home`.
2. The `pwd` command printed the current working directory (`/Users/stanley`).
3. We created a new directory called `TheAwesomeTutorial` with the `mkdir` command.
4. Within `TheAwesomeTutorial` directory, we create 3 files called `stanley`, `is`, `cool`, respectively.
5. With the `ls -l` command, we were able to get a detailed list all the files within this directory.
6. We moved all files (The `*` is a wildcard, it'll match everything) to the directory above (denoted by `..`).
7. We changed to a directory above the current, again with `..`.
8. We then removed the directory and the 3 files with the `rm -rf` command. The `-rf` stands for recursive with force, which is dangerous when deleting because it's gone forever.

<br />

### At this point in time..

You are now a practioner of the terminal. Regardless of whether you become a programmer or just want to be more efficient on a computer, these skills will translate to all parts of your life if you become fluent.
I
<br />

###Continuing your personal development

Here are some great resources for getting started with the Terminal:

- [Command Line Primer](http://lifehacker.com/5633909/who-needs-a-mouse-learn-to-use-the-command-line-for-almost-anything)
- [Basic Unix](http://mally.stanford.edu/~sr/computing/basic-unix.html)


<br />

###If you enjoyed this blog..

Let me know at <mailto:stanley@stanleycyang.com>

I would love to get you started coding with tips, advices, references. 