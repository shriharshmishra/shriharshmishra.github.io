---
layout: post
title: "Today I Learned: About Docker Volume"
published: true
description: A short introduction.    
tags: docker, volume
---

![Docker Volume](/assets/images/docker-volume.png "Docker Volume")



In my experiements on running Jenkins on Docker, I wondered how the volumne worked when I provided `-v` to the run command. 

I found a great article which succintly explains a lot about the Docker Volume - which will be handy for any docker beginer. 

Original article is [here][DockerVol]

My summary goes below: 

- Docker Volume is the way to _persist data between container restarts_ and _share it between containers_. 
- A volume is stored on the host file system. Usually within obscure directories inside `/var/lib/docker`. However, a specific directory on host system can also be mounted as volume in a container. 
- Volumes can be managed using `docker volume` and its sub commands. 
- Docker won't delete volume unless specifically instructed to do so. This can result in stale data on the host system. So tidying up once in a while might come handy. 
- **Important Caveat** - Don't try to modify the content of the volume in the Dockerfile (via RUN) as each build step creates a *new* volume and your changes will be discarded. If you want to change the volume content, do so using the *ENTRYPOINT* command in the Dockerfile. Have a look at this [link][SOL]. 
- If you are looking for a thorough overview of different approaches to manage application data, including the use of **Volume, Bind mounts, tmpfs mounts**, read this [longer article][Voluminous]



*Image Credit: https://docs.docker.com/engine/admin/volumes/images/types-of-mounts-volume.png*

[DockerVol]:http://container-solutions.com/understanding-volumes-docker/
[SOL]: https://stackoverflow.com/a/34843099/7007521
[Voluminous]: https://docs.docker.com/engine/admin/volumes/
