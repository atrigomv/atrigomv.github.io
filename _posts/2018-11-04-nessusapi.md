---
layout: post
title: Playing with Nessus API Rest
---
In addition to hacking challenges or other cybersecurity stuff, I enjoy a lot hardening new developments. As a Head of in New Initiatives in MAPFRE one of my main task is securize the software development in the company, both waterfall or Agile projects. So, one of the concepts which as a security member we have to know, is DevOps (or DevSecOps for us). There are a lot of definition of that term, but for me is securize the pipeline of developers and to offer to them cybersecurity services that they can use to improve the security of their projects.

One of the most important projects that I working on is, in point of fact, the construction of a new cybersecurity service which could be invoked to execute different scans within a develop pipeline. It is written in Python, as usual :), and one of the first services will be the execution of Nessus scans. To do this, I was working with the Nessus API Rest and below there are different points that anybody has to remember whether they want to work with it:
* The official [SDK](https://www.tenable.com/blog/tips-on-using-the-tenable-python-sdk-how-to-run-internal-scans-scan-imports-and-exports-and) is only to work with on cloud Nessus version. **You can not use it if you have on-premise (local) Nessus scanner.**
* You can find the API documentation in the folder "api" of your local scanner. For example in https://127.0.0.1:8834/api
* "POST" HTTP method is used to add new objects (scans, folders...), "GET" is used to list objects and "DELETE" to delete objects (easy? :P)
* All this requests has to be authenticated. To do this, the best method is using a pair of akey and skey instead of create new sessions with "POST session" method which is deprecated.
* To execute some methods you should attach as a payload the UUID identificator of your Nessus scanner. To retrieve it, you have to execute before it the next request:
```
r = r.get_post('https://127.0.0.1/scanner')
```

Finally, I've done a PoC script which configure a local Nessus scanner, create a new scanner, launch it and recover the scan_id for further actions. The script can be downloaded here.
