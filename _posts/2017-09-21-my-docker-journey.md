---
layout: post
title:  "My experiements with Vagrant, Docker and Jenkins"
date:   2017-09-21 16:38:42 +0000
categories: docker jenkins vagrant proxy
tags: docker jenkins vagrant proxy
---
#Docker with Vagrant.

#### 21-09-2017
First step was to assign docker as a provisioner in the Vagrantfile.

#### 22-09-2017

-  Ignore this step - Then install java image on using docker image command in Vagrant. 
-  Previous step isn't needed. The jenkins docker images uses 8-opendjdk as the base image. 
-  The jenkins image was setup to my vagrant box using the docker provisioner in Vagrantfile. But this wasn't straight forward. First I had to setup the proxy using the vagrant-proxyconf plugin. The update the vagrantfile with proxy info and finally add docker instructions in the proxy file. The standard approach to install vagrant-proxyconf plugin didn't work behind proxy(obviously). So I downloaded the gem file from [rubygems.org](https://rubygems.org/gems/vagrant-proxyconf) and use it to install the plugin.  
-  Once the vagrant machine is provisioned, log in and run the docker image. 
-  Follow the instruction on the [Jenkins image page](https://github.com/jenkinsci/docker) to start jenkins and use auto generated admin password to unlock jenkins. An important point is the ensure that the jenkins_home is bind mounted to host so that jenkins setup isn't lost between stop/restart.
-  Running jenkins behind corporate proxy has been a problem. I found a possible [solutions](https://blog.alexellis.io/jenkins-meets-the-proxy/) like setting up environment variable JAVA_OPTS on the jenkins image when calling docker run. But the solution didn't work for me. I tried setting up http_proxy variable on the running image using --env option on docker run but that didn't work either. For now, I have resorted to setting up jenkins image by running the image outside proxy and get things moving. This helped in installing all the recommended jenkins plugins. 

- The next step is the create a Dockerfile.

#### 28-09-2017 

- Studied Dockerfile reference. If I were to create an image, I know the steps now. But that's not what I need. My requirements is just that I want configure Vagrant to run Jenkins image and the ensuing container should expose the ports and persist Jenkins home directory across reboots. 

- I feel docker-compose.yml is what I need for now. Let me read how it can fit my solution.  

#### 04-10-2017

- Before I could start working on the docker-compose.yml, I wanted to strengthen my understanding of Docker so I read [more](https://github.com/docker/labs/tree/master/beginner).  But too much reading doesn't help anyway. So I shifted towards some hands-on. 

- I thought let me move a step forward towards my goal and decided to setup a Jenkins job to build my project using Apache Ant/Ivy.The job was setup, integration with SVN was done to ensure appropriate branch is selected during build as a parameter. 

    Howver I met with a road block again. First time the job was run, it tried download Apache Ant installation. As I was behind corporate proxy, I got stuck again. My Jenkins image didn't have the Ant installation. So once again I decided to get the networking sorted behind the proxy. 

- I decided to ready further about how to get Jenkins on Docker to run behind corporate proxy. I read few more articles on Docker  and Docker with Vagrant. I found a blog in which a gentleman Tom Friedhof was hashing out his experiments with Docker, Vagrant, Jenkins etc. [See here](http://activelamp.com/blog/devops/hashing-out-docker-workflow/).

- Finally, I had an epiphany. Just like I had my vagrant box talk to internet using http(s)_proxy environment variables, I can try doing the same to docker container. I tried passing the http_proxy and https_proxy environment variables to docker container using the following command. `docker run  --env http_proxy --env https_proxy --env no_proxy -p 18080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home  jenkins/jenkins:lts`

    I verified the internet connection on the container using the following steps - 
    - Login to the container `docker exec -it cd3da49eded2 /bin/bash`
    - Curl www.google.com - `curl -v www.google.com`. This worked!!

    But my problem wasn't solved as our corporate proxy allowed internet connection but blocked anonymous downloads. Jenkins was trying to download the Apache Ant zip file and proxy denied the permission to download it. 

    So finally the way forward for me is to extend the official Jenkins image and load it with necessary plugins and software.  


#### 20-10-2017

- Okay after a long gap, I resumed by journey on Docket + Jenkins. 
- Success Finally! Most of my last hurdles were due to project-specific environment setup such as host file entries, build tool installation, and problem around using Jenkins automatic installations over proxy etc.
- I found that I will have to install the Ant manually on the Jenkins image. For now I ssh'd to the container and installed Ant using apt-get. Final solution would be to extend the base Jenkins image and perform these settings via Dockerfile. 
- Overall I was happy to see the my Jenkins container was up and running builds successfully!
