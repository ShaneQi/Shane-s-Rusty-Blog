---
title: Config a Git Server with Gitolite
permalink: config-a-git-server-with-gitolite-16-01-21
date: 2016-01-21 14:58
---

> Based on Ubuntu 14.04

In order not to pay Github for keeping repo private, I built a git server on my VPS. The following is how I did that.

1. Install gitolite with a single line command:  
`sudo apt-get install gitolite`
2. create a user for gitolite to be accessed as:  
`sudo adduser --system --shell /bin/bash --group --disabled-password --home /home/git git`
3. Generate SSH keys from the host which you want to be the administrator:  
`ssh-keygen -t rsa`  
 When you are prompted for a file name and a password, strongly recommend simply hitting enter to generate with default name.  
 Because referring to my case, when I made a attempt to connect to the git server, it only try to fetch private ssh key with default file names.  
 So it is possibly that the private key with non-default-name is not caught to authenticate login.
4. Put the public part of the pair of SSH keys to a location that git user(the one I created in the second step) can access. And then run gitolite setup with it.  
 For example, I put the id_rsa.pub to /tmp/id_rsa.pub, so I need these commands to setup gitolite:  
```
sudo su - git<br></br>
gl-setup /tmp/id_rsa.pub```
5. For now, gitolite is all set for me but only me. In order to add other users, I need to clone gitolite-admin.git to the administrator host(these are supposed to be operated under the administrator host and administrator user):  
```
git clone git@$IP_ADDRESS:gitolite-admin.git<br></br>
cd gitolite-admin```
  
To add a new user, put new user’s pub SSH key into `gitolite-admin/keydir/`, then commit and push the gitolite-admin repo.  
To add a repo, append new repo info into `gitolite-admin/conf/gitolite.conf` then commit and push the gitolite-admin repo, before push the new repo to the server.
