---
layout: post
title: "DNS Terminology: Some things that matter to a casual tech blogger" 
date:   2018-01-09 
categories: domain, name servers, cname 
tags: DNS, CNAME, A, AAAA
---

Recently I read an article to clear the ambiguities about DNS and the process of lookup. Here are some of the new terms that I learned. 

Resolving Name Server: A server which on behalf of a user resolves the DNS domain name to an IP address by recursively seeking information form root server,Top Level Domain (TLD) and Domain-Level Name servers. 

Zone file: A file in which has a mapping of a domain and its IP addresss. $ORIGIN parameter in zone file or domain server configuration tells which domain's information can be found in this zone file. For e.g. a value of "example.com" means that the zone file can answer authoritively about example.com. 

Time To Live (TTL): Domain name servers will cache the name and IP address as per the time defined by this value. 

CNAME (Canonical Name) Record: This is a record in the zone file which maps a host to an alias. 
A: This is a record in the zone file which maps a host name to an IPv4 address. 
A: This is a record in the zone file which maps a host name to an IPv6 address. 
MX: is a record in the zone file which defines the mail server for the domain. 


The original tutorial/article which helped me learn the above concepts is [here](https://www.digitalocean.com/community/tutorials/an-introduction-to-dns-terminology-components-and-concepts). 
