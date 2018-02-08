---
layout: post
title:  "Today I Learned: A simple trick to slim down a Docker image"
date:   2018-02-08 10:38:42 +0000
categories: docker
tags: docker, ubuntu, tips, tricks 
---


You may want to reduce the space used by your image. If you are building on top of an  Ubuntu image and during build you are invoking `apt-get update` (you would be for sure if you are installing latest versions!) then you can delete the files downloaded by apt-get. The following Dockerfile snippet shows how it should be done. Notice that RUN instruction is doing 3 things, update the package cache, installing ant, and deleting the cache. This is done in such a ways so that Docker does all of this in a single layer of the image. If these steps are split in different RUN instructions then your Docker host and/or registry will use more space.   

```shell

FROM jenkins/jenkins:lts

#Change user to root and install ant version 1.9.9-1 (available in Ubuntu packages)
USER root

#
RUN apt-get update && \
        apt-get install -y ant=1.9.9-1 && \ 
        rm -rf /var/lib/apt/lists/* # remove the cached files.

```

In my case, it saved about 138M of space. However, YMMV. 

By the way, __what are you deleting__? You are only deleting the information about latest packages and versions available for download using `apt-get install` or `apt-get upgrade`. `apt-get` caches this information to use it later. It is highly likely that you wouldn't need this information when you are run the containers based on this image. Installing software while running the container would be a big NO!. 

Similar, cache files would exist in other Linux distributiions as well. 

I hope this helps you slim down your ubuntu images. 

