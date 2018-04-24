---
layout: post
title:  "Vim Trick: Search In A Range"
date:   2018-01-23 15:38:42 +0000
categories: vim, til
tags: vim, til 
---

Unfortunately, I was dealing with some badly written legacy code. Some of the files with core business logic had _more than 1000 line of code_. I wanted to search for a word within a method. I did the usual Vim search `/searched-word`. As I cycled thru the search results, I noticed that the results were spanning across multiple methods. As the methods were so large that they wouldn't fit on the screen, I didn't realize when I had left the boundary of the current method.  

To my rescue was search-range help text - `h search-range`

There are two options 

#### Option 1 - Provide a line number range to `/` command

For example, to search the word `test` between line 30 and 500, type the following command in normal mode. 
```
    /\%>30l\%<500ltest
```
This is nothing but the usual `/` search command with a regex pattern that provides a range of line numbers. Let's see what it contains: 

- `\%>30l` (`\%>l`) is an special sequence (called atom)  which Vim's regex engine interprets as _greater than line number_. `30` in our example is the line number. 
- `\%<500l` (`\%<l`) is a special sequence which is interpreted as _less than line number_. 
- `test` is our search text.

This option is bit tedious to type and read. 

#### Option 2 - Use marks as boundaries in `/` command

```
    /\%>'s\%<'etest
```
This is very similar to our previous option but doesn't require you to remember or read line numbers. Again it uses special sequeneces to.
