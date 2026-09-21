---
title: Host Multiple Websites with nginx
permalink: host-multiple-websites-with-nginx-16-03-07
date: 2016-03-07 12:29
---


This article is about how to host multiple websites with **nginx**.

The easiest way to do that is putting all the websites into different directories and put the directories in the server root location, so that they can be accessed by subdirectory.

For example, let’s say I have a blog. I put it in ‘blog’ directory and put the directory in the root directory. Then when I want to get accessed to this blog, I need go to the url: MyDomain.com/blog.

That’s okay, getting access to multiple websites with Subdirectory.

However, what I want to talk about here is host multiple web servers with **nginx** to implement multiple websites which means you can put your websites any where on your server and you can get access to them with any domain you want.

Firstly, you have to make sure that the domain you want to use can resolved to your server. In my case, I want to use subdomain blog.MyDomain.com to get access to my blog site, so I put a CNAME record to my domain, directing blog.MyDomain.com to my server IP address.

Then what you should do is add an server nginx configuration.

Nginx configurations are typically stored in /etc/nginx/sites-enabled. Open the file there and add a server block like this:

 server { listen 80; root /home/shane/blog/; server_name blog.zhanga.ru; autoindex on; index index.php index.html index.htm; location ~ \.php$ { try_files $uri =404; fastcgi_split_path_info ^(.+\.php)(/.+)$; fastcgi_pass unix:/var/run/php5-fpm.sock; fastcgi_index index.php; fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name; include fastcgi_params; } }

These three lines are typically what you should modify:

1. `root /home/shane/blog/;`  
 Modify to the location of your website directory.
2. `server_name blog.zhanga.ru;`  
 Modify to the domain address you want to use.
3. `fastcgi_pass unix:/var/run/php5-fpm.sock;`  
 Modify to your location of php sock file.

In my case, after save the configuration and restart nginx, I can get access to the website which is located in /home/shane/blog/ of my server using http://blog.zhanga.ru, and it will run the php files with /var/run/php5-fpm.sock.



