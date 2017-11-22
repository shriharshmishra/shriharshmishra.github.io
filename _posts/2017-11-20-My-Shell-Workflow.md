---
layout: post
title:  "My Shell Workflow: Using bash history"
date:   2017-11-20 14:15:42 +0000
categories: shell bash zsh 
tags: shell bash zsh 
published: true
---

### Working on the shell with your hands tied :|

While I try to use my vagrant box for work as much as possible, there are times when I have to connect to server machines over ssh and not have my favorite z-shell. It was getting irritatingly boring to repeat the same set of command every time. One option is to create my own aliases but I will have to do so everytime I login. Our sysadmin has (strangely) ensured that everyone sudo's to a common user to do any editing work. 

### Finding my way out :) 
So I use the following set of commands to ensure that I am not getting bogged down by the process. 

Suppose I want to search a command that I have previously used. I type  `history | grep <command-snippet>` which gives me a numbered list when I have used the command earlier. Then I use the number to run the same command - `!<number>` again. See below - 

```shell
> history | grep vim
2795  vim scratch-pad.txt
2820  vim .git/config
2822  vim .git/config
2845  sudo vim /etc/hosts
2847  sudo vim /etc/hosts

> !2820
> vim .git/config
```

This might still be tedius. So the other option is to search history without calling `history` command. 

### The Savior `Ctrl+R` :D

- On the bash terminal, press `Ctrl+R` to open reverse search dialog. 
- Type the snippet of the command you would have executed earlier. 
- Press `Ctrl+O` to execute the command. or Press `Ctrl+Space` to print the command for editing before executing. 

```shell
(reverse-i-search)`install': sudo apt-get install ant #After pressing Ctrl+O
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following additional packages will be installed:
.... 


(reverse-i-search)`zsh': sudo apt-get install zsh #After pressing Ctrl+Space
> sudo apt-get install zsh  
```

A nifty side-effect of using `Ctrl+R` on the bash prompt is that once the chosen command is completed, the next command from history is printed on the prompt for us to use if needed. Most of the time, I am restarting my server and tailing the logs. So using this option automatically returns the next command (`tail -f`) after I execute the restart command. 

I hope this helps you as well. 
