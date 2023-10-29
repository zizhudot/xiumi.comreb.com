---
title: How to Implement Gateway Services with Nginx on Low-Code Platforms
description: How to Implement Gateway Services with Nginx on Low-Code Platforms
tags:
  - IaaS
  - cloud
  - cloud computing
date: 2023-10-29 19:50:26
---

**Preface**

In a typical system deployment architecture, an application server is a software or hardware system that carries the core logic of an application program. It receives requests from clients and handles the corresponding business logic, data manipulation, and other tasks. Application servers are typically used to support Web applications, mobile applications, enterprise applications, and so on. On top of the application server is usually the gateway server, and below it is the database service. Interestingly, in low-code platforms, there are also application servers. Today, I will take GrapeCity's enterprise-grade low-code development platform - [living word grid]() as an example to introduce the auxiliary role of gateway servers for low-code platforms. ! [](https://img2023.cnblogs.com/blog/139239/202309/139239-20230927103026740-1645389022.png)

**Application Scenarios Realized with Nginx**

In this article, the gateway server Nginx will be used as an example to show four scenarios of gateway services:

1. cross-domain access: allow multiple applications to share the same server port.
2. Static resources: authentication via WeChat public platforms, etc.
3. IP black and white list: to meet higher security requirements.
4. access logs: detailed records and analyze system responsiveness.

**1. Cross-domain access: allow multiple applications to share the same port on the same server **

Splitting multiple modules of the same system into a number of applications is a highly recommended practice pattern for both development management and system operation and maintenance. However, if the front-end page of one application needs to call the server-side commands of another application, it will encounter the problem of cross-domain access. In order to solve this problem, we need to move all these cross-application calls to server-side commands: the front-end page of application A calls the server-side commands of application A, and the server-side commands call the WebAPI of application B. This approach adds to the workload of developing the server-side commands of application A, and there will be additional work and risk in the later maintenance. ! [](https://img2023.cnblogs.com/blog/139239/202309/139239-20230927103043740-473949187.png)

In coded development, this cross-domain access problem is usually avoided by unifying all applications into the same address and port through a gateway. And in low-code development, the solution is the same. ! [](https://img2023.cnblogs.com/blog/139239/202309/139239-20230927103059117-1118130201.png)

Set up an Nginx server on the server and use multiple applications as upstream of Nginx. the specific configuration is as follows:

(1) Modify the Nginx configuration to configure an upstream node below the http node for the management console and each application, containing the machine name and port number. In the test environment, Nginx is installed on the application server, so here you can directly use localhost. in general, Nginx needs to be deployed to the application server and another server in the same LAN, this time, you need to replace localhost with the server's intranet IP.

Tip: Adding upstream to the application server instead of jumping directly in the location improves the readability of the configuration file.

```nginx
upstream us-server{
     server localhost:22345;
   }

   upstream red-server{
     server localhost:9101; }
   }

   upstream green-server{
     server localhost:9102; }
   }
```

(2) Listen on port 80 or other specified port number in the http→server node.

``` nginx
listen 80; }
```

(3) In the http→server node, configure a location node for the management console and for each application, containing the rules for matching URLs and the corresponding upstreams. the most commonly used rule is to match from the beginning, i.e. ^~ means to start with the string that comes after it, e.g. location ^~ /red/ matches all locations that start with /red/ (URLs that start with /red/), and the most commonly used rule is to match from the end. (the part of the URL after the port number).

```nginx
 location ^~ /UserService/ {
       proxy_pass http://us-server/UserService/;
       proxy_redirect default;   
     }
 
 location ^~ /red/ {
   proxy_pass http://red-server/red/; proxy_redirect default; }
   proxy_redirect default; }
 }
 
 location ^~ /green/ {
   proxy_pass http://green-server/green/; proxy_redirect default; }
   proxy_redirect default; }
 } 
```

(4) On the Live Grid Management Console, change the application's "Domain Name" (Application Management → Applications → General Settings → Set Domain Name) to the port on which Nginx listens to ensure that the external navigation of the page works correctly. The configuration of the application needs to include the application name.

! [](https://img2023.cnblogs.com/blog/139239/202309/139239-20230927103156228-1909080014.png)

The "domain name" of the management console also needs to be set (Settings→Security Settings→Set the domain name of the management console site), and the configuration of the console does not include the application name. The console configuration does not include the application name. [](https://img2023.cnblogs.com/blog/139239/202309/139239-20230927103207817-1202034282.png)

Extended Scenarios:  
If your IT security policy requires that only ports 80/443 can be opened, but you need to access to multiple applications, you can also use this scenario to achieve port unification.

**2. Static resources: authentication via WeChat public platform, etc.**

When docking WeChat public platform and other third-party systems, the other party usually proposes a file-based domain name verification mechanism, such as WeChat public platform's JS interface domain name verification needs to be put into the root directory of the domain name of a specific file. ! [](https://img2023.cnblogs.com/blog/139239/202309/139239-20230927103227297-1612904409.png)

At this time, you can use the gateway's static resource server capability to complete the verification work.

! [](https://img2023.cnblogs.com/blog/139239/202309/139239-20230927103254297-713067348.png)

Continue to configure the Nginx file on the basis of [1. Cross-domain access] as follows:

(1): In the interface of domain name management, point the domain name filed through ICP to the external network address of Nginx server.

(2): Store the static files that need to provide access to the outside world in the Nginx static resource root directory on the Nginx server, such as /etc/Nginx/html (the root directory may also be /usr/share/Nginx/html or /var/www/html for different installations and versions; by default, the Nginx root directory has By default, the Nginx root directory has two files: index.html and 50x.html.) Nginx makes the files in the root directory available for external use as static Web resources. Specifically, when receiving a request with the URL /xxx.yyyy, Nginx returns the contents of the xxx.yyyy file in the root directory as a response.

! [](https://img2023.cnblogs.com/blog/139239/202309/139239-20230927103311155-1028781805.png)

**3. IP Black/White List: Satisfying Higher Security Requirements

For some application scenarios with high security requirements, it is often required to do black and white lists, such as allowing only specific IPs to access, or disallowing a specific IP to access. These tasks are recommended to be performed on the gateway to block the risk outside the application server.  
Because the management console built into Live Grid contains sensitive operations such as application management, user role management, and so on, many organizations require that whitelisting controls be enabled for this application, allowing access only from IP addresses dedicated to the company's IT operations team. Next, we continue to improve the Nginx configuration to achieve whitelisting on the basis of [II, cross-domain access]. Specific methods of operation are as follows:

Modify the Nginx configuration in the http ¡ú server node to find the corresponding location of the management console, append the following content below to add the intranet 10.32.209.252 and extranet 113.132.178.118 to the whitelist.

```nginx
 location ^~ /UserService/ {
       proxy_pass http://us-server/UserService/;
       proxy_redirect default;  
       allow 10.32.209.252;
       allow 113.132.178.118;
       deny all; } 
     }
```

Important Tip:  
The whitelisting level at the gateway level is higher than the system firewall, and the two are not substitutes. You still need to use firewall policies to avoid exposing unnecessary ports to reduce security risks.

**4. Access Logs: Detailed Records and Analysis of System Responsiveness **

When you need to evaluate the system's response performance, availability and other parameters, looking for improvement, you need to record the application access logs through a third party, and then connect it to the mainstream log processing and analysis tool chain (log analysis is a "high-tech" field, there are already mature solutions, such as ELK) for follow-up processing.

! [](https://img2023.cnblogs.com/blog/139239/202309/139239-20230927103329786-879085603.png)

The good news is that Nginx has a built-in logging mechanism, you only need to do a very simple configuration, you can get the desired logs, and then follow the ELK help file, you can get your own log analysis platform. Continue to continue to improve the Nginx configuration and logging configuration on the basis of [3. IP black and white list]. Specific methods of operation are as follows:

(1): Modify the Nginx configuration in the http node to add a log template named json for filebeats to grab.

`` nginx
log_format json escape=json '{"time_local":"$time_local", '
              '"remote_addr":"$remote_addr", '
              '"request_uri":"$request_uri", '
              '"status": $status, '
              '"upstream_time": "$upstream_response_time"}'; '"upstream_time": "$upstream_response_time";'
```

(2): Modify the http→server node to append the configuration of the access log, specifying the file path and the template access\_log /var/log/Nginx/access.log json just defined named json; ! [](https://img2023.cnblogs.com/blog/139239/202309/139239-20230927103341826-1640239524.png)

Commonly used log items and parameters are shown below: ! [](https://img2023.cnblogs.com/blog/139239/202309/139239-20230927103353073-707490179.png)

**Summary**

The configuration files used in this article are linked as attached, using the simplest configuration. Among them, worker\_processes and worker\_connections are related to resource usage and performance. Please make appropriate adjustments according to the machine configuration.