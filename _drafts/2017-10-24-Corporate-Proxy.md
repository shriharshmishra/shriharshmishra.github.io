---
layout: post
title:  "Behind the Coporate Proxy"
date:   2017-10-24 16:38:42 +0000
categories: proxy shell vagrant docker  
tags: til proxy shell vagrant docker 
---

During my experiments Docker, Jenkins and Vagrant, I struggled getting past the corporate proxy. I have been able to resolve some of them of my problems and thought I have some ammunition for a short [TIL] post.  

### Running Vagrant behind the proxy 

There are two options, that I know: 

- **Set up proxy in your guest OS**: Depends on the guest OS. I use Ubuntu, so I find updating `/etc/environment` file with proxy details as an easy option. 
- **Use vagrant-proxyconf plugin**: With this plugin you can setup the proxy details right into the Vagrantfile. For more details [see][v-proxy-conf]

    If you are not able to download the plugin behind corporate proxy (duh!), then you can download the gem file from [Ruby Gems] site and use the following command to install the plugin. 
    ```shell
    vagrant plugin install /path/to/my-plugin.gem
    ```

### Running Docker behind proxy
[v-proxy-conf]: http://tmatilai.github.io/vagrant-proxyconf/ 
[Ruby Gems]: https://rubygems.org/gems/vagrant-proxyconf/versions/1.5.2
[TIL]: /
