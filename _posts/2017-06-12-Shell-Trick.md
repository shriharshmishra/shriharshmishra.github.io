---
layout: post
title:  "My Shell Workflow: Executing a command in lots of directories."
date:   2018-01-03 15:38:42 +0000
categories: shell,bash 
tags: bash, shell
---

I wanted to execute a shell command in lots of directories(approx. 200)  to clean up build files. Writing a small for loop did the trick for me.  
Here, I am sharing it with you all. 


```
for d in ./*/ ; do (cd "$d" && mvn clean); done
```

The above for-loop would invoke  `mvn clean` in each folder of the directory list defined by the path `./*/`.

I hope this help you as well.
