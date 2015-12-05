# GitBook Docker

## Introduction

Gitbook docker helps to run gitbook without installing nodejs or anything.

The [container]() is based on Ubuntu 14.04 (phusion/baseimage) and gitbook 2.6.4.

## Usage

Build the docker image:

```
cd /PATH_DOCKERFILE
docker build -t "gitbook-docker" .
```

Clone a gitbook project in a know path `/home/yourname/yourbook` or anywhere, then run this command.

```
cd /opt
git clone git@github.com:GitbookIO/documentation.git
```

Then run this command:

```
docker run -d -t -i -p 4000:4000 -p 2022:22 -p 35729:35729 -v /opt/documentation:/opt/gitbook gitbook-docker
```

## SELINUX Error

On Fedora or any Linux computer with SELINUX as your Docker Server: 
If you get run `docker logs container_name` and the log appears like 
```
Press CTRL+C to quit ...

Live reload server started on port: 35729
Starting build ...
EACCES, open '/gitbook/README.md'
```

It is most likely caused by the SELINUX settings restricting the container from accessing the folder. To add a rule to allow the container access to yourbook folder as root run:
 
```
chcon -Rt svirt_sandbox_file_t /home/yourname/yourbook/
```

You should be able to start the container.
Kensel found the answer to this problem [Here] (http://stackoverflow.com/questions/24288616/permission-denied-on-accessing-host-directory-in-docker)

Based on: https://github.com/tobegit3hub/gitbook-server

