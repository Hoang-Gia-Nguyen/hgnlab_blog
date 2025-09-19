+++
title = "More people should know about jump and thefuck"
date = 2025-09-19
draft = false
description = "Discover two life-changing terminal tools, jump and thefuck, that will save you countless keystrokes and fix your command-line mistakes."
[taxonomies]
categories = ["Technology"]
+++

If you're a software engineer, a student, or just someone who spends a lot of time in the terminal, you know that efficiency is key. Shaving off a few seconds here and there adds up, but more importantly, it keeps you in the flow. Today, I want to share two small but mighty command-line tools that have become indispensable in my daily workflow: `autojump` and `thefuck`.

## `autojump`: The Teleporter for Your Filesystem

Tired of typing `cd ../../somet/long/path/you/visit/often`? I certainly was.

**What is it?**
[`autojump`](https://github.com/wting/autojump) is a faster way to navigate your filesystem. It learns your habits and allows you to jump to your favorite directories with a simple command.

**How it works**
`autojump` maintains a database of the directories you visit. It ranks them by "frecency"—a combination of frequency and recency. So, the directories you use most often and most recently get higher scores. When you want to jump, you just provide a part of the directory's name, and `autojump` teleports you to the best match.

**Example**
Let's say you often work in `<very_long_path>/Workspace/hgnlab_blog/content`. After `cd`-ing into it a few times, `autojump` will learn it. From then on, no matter where you are in the filesystem, you can just type:

```bash
j blog
```

And it will take you straight to `<very_long_path>/Workspace/hgnlab_blog/content`. It's a huge time-saver!

## `thefuck`: Your Universal Undo Button

We all make typos. `git psuh`, `puthon main.py`, `apt-get isntall...` It's frustrating to re-type the whole command just to fix a small mistake.

**What is it?**
[`TheFuck`](https://github.com/nvbn/thefuck) is a magnificent application that corrects errors in your previous console commands.

**How it works**
When you run a command and it fails, you can just type `fuck`. `TheFuck` then inspects the last command, identifies the error using a set of configurable rules, and suggests a corrected command. You can then approve the suggestion, and it will be executed.

**Examples**

> Mistyped git command
```bash
$ git psuh
git: 'psuh' is not a git command. See 'git --help'.

Did you mean this?
        push

$ fuck
git push [enter/↑/↓/ctrl+c]
```

> Forgot `sudo`
```bash
$ apt-get install vim
E: Could not open lock file /var/lib/dpkg/lock-frontend - open (13: Permission denied)
E: Unable to acquire the dpkg frontend lock (/var/lib/dpkg/lock-frontend), are you root?

$ fuck
sudo apt-get install vim [enter/↑/↓/ctrl+c]
```

It feels like magic and never fails to make me smile.

---

Both `autojump` and `thefuck` are small utilities that punch way above their weight. They streamline your workflow, reduce frustration, and save you a surprising amount of time. Give them a try!
