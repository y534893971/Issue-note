# Background

最近在加了一个jenkins slave 节点， 是 ubuntu 系统， 之前是在master 节点build centos 系统。 当运行的时候，提示如下报错:

```
11:05:22  + source example/stg/init.sh
11:05:22  example/stg/example.sh: 2: source: not found
```

由于 source 是 build-in command, 因此不存在未安装的问题。
Jenkinsfile 内容如下

```
sh """
    #!/usr/bin/env bash
"""
```

# Solution

```
jenkins@:~$ ls -l `which sh`
lrwxrwxrwx 1 root root 4 Jan 10  2023 /usr/bin/sh -> dash
jenkins@:~$ sudo dpkg-reconfigure dash
Removing 'diversion of /bin/sh to /bin/sh.distrib by dash'
Adding 'diversion of /bin/sh to /bin/sh.distrib by bash'
Removing 'diversion of /usr/share/man/man1/sh.1.gz to /usr/share/man/man1/sh.distrib.1.gz by dash'
Adding 'diversion of /usr/share/man/man1/sh.1.gz to /usr/share/man/man1/sh.distrib.1.gz by bash'
jenkins@:~$ ls -l `which sh`
lrwxrwxrwx 1 root root 4 Dec 12 12:16 /usr/bin/sh -> bash
jenkins@:~$ ls -l `which sh`
lrwxrwxrwx 1 root root 4 Dec 12 12:16 /usr/bin/sh -> bash
jenkins@:~$
```
将sh 链接到 bash