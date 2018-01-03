---
layout: post
title:  "Learning Security: SSL, Certificates and keytool"
date:   2017-06-16 16:38:42 +0000
categories: SSL, TLS, Security, Certificate 
tags: SSL, TLS, Security, Certificate
---

### What is a certificate? 

Certificate is a file which contains the following information to identify a sender in a reliable manner. 
- Public Key of the sender
- Distinguished Name of the sender. This is name, address, organisation, location etc. 
- Digital Signature - encrypted text which helps in identifying that the issuer has issued the certificate. 
- Distinguished Name of the issuer. 


### What is keytool?
In Java world, `keytool` is a utility which is used to create and manage certificates. The keytool tool can be used to -
- Create private keys and their associated public key certificates
- Issue certificate requests, which you send to the appropriate certification authority
- Import certificate replies, obtained from the certification authority you contacted
- Import public key certificates belonging to other parties as trusted certificates
- Manage your keystore


For more infomation on keytool, [see this](https://docs.oracle.com/javase/tutorial/security/sigcert/index.html#GenCSR)
The following page also gives a simple overview of `keytool` features - [`keytool` Overview](https://www.sslshopper.com/article-most-common-java-keytool-keystore-commands.html)
