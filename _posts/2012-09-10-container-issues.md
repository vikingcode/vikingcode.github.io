---
layout: page
title: Team City build agent certificate issues
date: 2012-09-10
---

For posterity sake since every time I set up a build agent I forget this. When faced with the error: 

>Cannot import the following key file: *whatever*.pfx. The key file may be password protected. To correct this, try to import the certificate again or manually install the certificate to the Strong Name CSP with the following key container name: *containername*

The solution is to run the VS Command Prompt as admin and issue

>sn -i *whatever*.pfx *containername*