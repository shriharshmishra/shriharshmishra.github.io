---
layout: post
title:  "Behind the Corporate Proxy"
description: 
date:   2018-01-12 12:38:42 +0000
categories: shell proxy  
tags: til proxy shell vagrant docker 
---

Over the years, I have struggled getting past the corporate proxy. These are useless battles a developer has to fight in order to win the war. I have been able to win some of my battles and have accumulated some ammunition (unlike real wars <sup>[1](#info-war)</sup>)  for a short post which might come handy to many in similar situation.  

### Running Vagrant behind the proxy 

There are two options, that I know: 

- **Set up proxy in your guest OS**: Depends on the guest OS. I use Ubuntu, so I find updating `/etc/environment` file with proxy details as an easy option. 

    ```shell
    > cat /etc/environment
    PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games"

    HTTP_PROXY=http://proxy_host:port
    HTTPS_PROXY=http://proxy_host:port
    FTP_PROXY=
    NO_PROXY=localhost,127.0.0.1
    http_proxy=http://proxy_host:port
    https_proxy=http://proxy_host:port
    ftp_proxy=
    no_proxy=localhost,127.0.0.1

    ```

- **Use vagrant-proxyconf plugin**: With this plugin you can setup the proxy details right into the Vagrantfile. For more details [see][v-proxy-conf]

    If you are not able to download the plugin behind corporate proxy (duh!), then you can download the gem file from [Ruby Gems] site and use the following command to install the plugin. 
    ```shell
    vagrant plugin install /path/to/my-plugin.gem
    ```

### Running Docker behind proxy

When you are working on docker behind proxy, it becomes important that you ensure proxy settings are passed and are availabe at different stages -  for example,  image creation, and container execution. There are different command options for either of these scenarios.  

-  **Setting proxy while building an image**:
You might want to download some software while building your image so that every container has it pre-installed. While you can hardcode the proxy details as environment variables in the Dockerfile, it isn't the recommended way of doing so. It hampers the portability of the image and unnecessarily gives the container created by the image access to internet. The better option would be to pass the proxy information while building the image using `build-arg` option as shown below: 


    ```shell
    > http_proxy=<yourproxyserverhost>:<port>
    > https_proxy=<yourproxyserverhost>:<port>
    > docker build --build-arg http_proxy=$http_proxy --build-arg https_proxy=$https_proxy -t yourimage .
    ```

    The `build-arg` option would allow you to pass a value to an environment variable defined in your Dockerfile using `ARG` directive. However, it is bit easier to pass proxy details. For proxy details, Dockerfile allows you to skip mentioning the proxy environment variable via `ARG` directive in the Dockerfile. Refer to the documentation for more details: [build-arg reference]

- **Setting up proxy when running the container**: 
Again as mentioned above, it isn't advisable to setup proxy details in the image or enter each new container to setup proxy details. Docker provides a better way to do so using the `-e` or `--env` options with the run command. This option can be used to pass the proxy setting as environment variables to the container.  

    ```shell
    > docker container run -e http_proxy nginx #..........................(1)
    > docker container run -e https_proxy=<proxyhost:port nginx #.........(2)
    ```

    Notice the difference between two commands above. If `http_proxy` is already defined on the docker host, we can directly pass its value to the container by just passing the name of variable to `-e` options as shown on line (1). If you want to provide an alternate value you can pass name=value pair as shown in line (2). Finally, you can also setup all the proxy variables in a file and use the `--env-file` option to pass all of them at once.
    
    ```shell
    > cat all-proxy-vars.txt 

    http_proxy=http://proxy_host:port
    https_proxy=http://proxy_host:port
    ftp_proxy=
    no_proxy=localhost,127.0.0.1

    > docker container run --env-file ./all-proxy-vars.txt nginx 
    ```

    You can find more details on passing the environment variables to containers here: [env option reference]

So these are the corporate proxy battles that I have fought and won some how. A long time back, I had a hard time getting thru an NTLM-based proxy. But a cool tool called CNTLM saved my day. However, I don't have the scenario or the steps handy to add to this article. I would add to this article, when I fight and win more such battles. I hope you this post helps you save some time. 

[env option reference]: https://docs.docker.com/engine/reference/commandline/run/#set-environment-variables--e-env-env-file
[build-arg reference]: https://docs.docker.com/engine/reference/commandline/build/#set-build-time-variables-build-arg
[v-proxy-conf]: http://tmatilai.github.io/vagrant-proxyconf/ 
[Ruby Gems]: https://rubygems.org/gems/vagrant-proxyconf/versions/1.5.2
<a name="info-war">1</a>: In the knowledge warfare, ammunition (i.e. information) increases as one ventures further! 
