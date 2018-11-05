---
layout: post
title: Playing with Nessus API Rest
---
In addition to hacking challenges or other cybersecurity stuff, I enjoy a lot hardening new developments. As a Head of in New Initiatives in MAPFRE one of my main task is securize the software development in the company, both waterfall or Agile projects. So, one of the concepts which as a security member we have to know, is DevOps (or DevSecOps for us). There are a lot of definition of that term, but for me is securize the pipeline of developers and offer to them cybersecurity services that they can use to improve the security of their projects.

One of the most important projects that I working on is, in point of fact, the construction of a new cybersecurity service which could be invoked to execute different scans within a develop pipeline. It is written in Python, as usual :), and one of the first services will be the execution of Nessus scans. To do this, I was working with the Nessus API Rest and below there are different points that anybody has to remember whether they want to work with it:
* The official [SDK](https://www.tenable.com/blog/tips-on-using-the-tenable-python-sdk-how-to-run-internal-scans-scan-imports-and-exports-and) is only to work with on cloud Nessus version. **You can not use it if you have on-premise (local) Nessus scanner.**
* You can find the API documentation in the folder "api" of your local scanner. For example in https://127.0.0.1:8834/api
* "POST" HTTP method is used to add new objects (scans, folders...), "GET" is used to list objects and "DELETE" to delete objects (easy? :P)
* All this requests has to be authenticated. To do this, the best method is using a pair of akey and skey instead of create new sessions with "POST session" method which is deprecated.
* To execute some methods you should attach as a payload the UUID identificator of your Nessus scanner. To retrieve it, you have to execute before it the next request:
```
r = requests.request('GET','https://127.0.0.1:8834/editor/scan/templates',verify=False, headers=headers)
```
* In order to create a new scan you have to set some variables. One of them is the Policy Scan. For our scanner, we set this variable as "Basic Network Scan" which its internal id number is 4. You can retrieve this value, and others, with the next HTTP request:
```
r = requests.request('GET','https://127.0.0.1:8834/policies',verify=False, headers=headers)
```
* The folder in which the results of our scan will be stored has to be set too. To see the internal id of that folders you have to execute previously the next request:
```
r = requests.request('GET','https://127.0.0.1:8834/folders',verify=False, headers=headers)
```
I´ve done a little piece of code that retrieve the ID of a specific Nessus folder or create it if it doesn´t exist. I´ve not incorporated it this code in the PoC but is the next one:
```
try:
                folder_id = 0
                r = requests.get(url + 'folders', verify=False, headers=headers)
                json_data = json.loads(r.text)
                folders = json_data['folders']
                for i in folders:
                        if i['name'] == folder_name:
                                folder_id = i['id']

                ### Si la carpeta no existe, la crea:
                if folder_id == 0:
                        payload = {"name": folder_name}
                        r = requests.post(url + 'folders', data=json.dumps(payload), verify=False, headers=headers)
                        json_data = json.loads(r.text)
                        folder_id = json_data['id']

        except Exception as ex:
                ret = {
                        'status': 'ERR',
                        'data': {
                                'exited': False,
                                'reason': 'Error retreaving folder_id.' + str(r.status_code), str(r.reason))
                        }
                }
                return ret
```

Finally, I've done a PoC script which configure a local Nessus scanner, create a new scanner, launch it and recover the scan_id for further actions. The script can be downloaded [here](https://raw.githubusercontent.com/atrigomv/devsecops/master/devsecops.py).
