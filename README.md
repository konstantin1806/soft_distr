СПИСОК КОМАНД И РЕЗУЛЬТАТ ВЫПОЛНЕНИЯ:

[konstantin@localhost ~]$ sudo yum install -y wget rpmdevtools rpm-build createrepo  yum-utils cmake gcc git nano

[konstantin@localhost ~]$ mkdir rpm && cd rpm


[konstantin@localhost rpm]$ yumdownloader --source nginx


[konstantin@localhost rpm]$ rpm -Uvh nginx*.src.rpm


[konstantin@localhost rpm]$ sudo yum-builddep nginx


[konstantin@localhost rpm]$ cd /home/konstantin/


[konstantin@localhost SPECS]$ sudo nano  nginx.spec


добавил: --add-module=/home/konstantin/ngx_brotli \


[konstantin@localhost SPECS]$ rpmbuild -ba nginx.spec -D 'debug_package %{nil}'

результат выполнения:

64.rpm
Wrote: /home/konstantin/rpmbuild/RPMS/noarch/nginx-filesystem-1.20.1-22.el9.3.alma.2.noarch.rpm
Wrote: /home/konstantin/rpmbuild/RPMS/noarch/nginx-all-modules-1.20.1-22.el9.3.alma.2.noarch.rpm
Wrote: /home/konstantin/rpmbuild/RPMS/x86_64/nginx-mod-devel-1.20.1-22.el9.3.alma.2.x86_64.rpm
Executing(%clean): /bin/sh -e /var/tmp/rpm-tmp.9LFVVZ
+ umask 022
+ cd /home/konstantin/rpmbuild/BUILD
+ cd nginx-1.20.1
+ /usr/bin/rm -rf /home/konstantin/rpmbuild/BUILDROOT/nginx-1.20.1-22.el9.3.alma.2.x86_64
+ RPM_EC=0
++ jobs -p
+ exit 0


[konstantin@localhost x86_64]$ ls
nginx-1.20.1-22.el9.3.alma.2.x86_64.rpm


[konstantin@localhost RPMS]$ cp noarch/* /home/konstantin/rpmbuild/RPMS/x86_64/


[konstantin@localhost x86_64]$ sudo yum localinstall *.rpm


[konstantin@localhost x86_64]$ systemctl start nginx

[konstantin@localhost x86_64]$ sudo systemctl start nginx
● nginx.service - The nginx HTTP and reverse proxy server
     Loaded: loaded (/usr/lib/systemd/system/nginx.service; disabled; preset: disabled)
     Active: active (running) since Wed 2026-03-04 11:39:57 MSK; 10s ago


[konstantin@localhost html]$ sudo createrepo /usr/share/nginx/html/repo/
Directory walk started
Directory walk done - 10 packages
Temporary output repo path: /usr/share/nginx/html/repo/.repodata/
Preparing sqlite DBs
Pool started (with 5 workers)
Pool finished


[konstantin@localhost html]$ sudo nginx -t
[sudo] password for konstantin:
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful


[konstantin@localhost html]$ sudo nginx -s reload


[konstantin@localhost html]$  curl -a http://localhost/repo/
<html>
<head><title>Index of /repo/</title></head>
<body>
<h1>Index of /repo/</h1><hr><pre><a href="../">../</a>
<a href="repodata/">repodata/</a>                                          04-Mar-2026 11:22                   -
<a href="nginx-1.20.1-22.el9.3.alma.2.x86_64.rpm">nginx-1.20.1-22.el9.3.alma.2.x86_64.rpm</a>            04-Mar-2026 11:22               36910
<a href="nginx-all-modules-1.20.1-22.el9.3.alma.2.noarch.rpm">nginx-all-modules-1.20.1-22.el9.3.alma.2.noarch..&gt;</a> 04-Mar-2026 11:22                8021
<a href="nginx-core-1.20.1-22.el9.3.alma.2.x86_64.rpm">nginx-core-1.20.1-22.el9.3.alma.2.x86_64.rpm</a>       04-Mar-2026 11:22             1029568
<a href="nginx-filesystem-1.20.1-22.el9.3.alma.2.noarch.rpm">nginx-filesystem-1.20.1-22.el9.3.alma.2.noarch.rpm</a> 04-Mar-2026 11:22                9623
<a href="nginx-mod-devel-1.20.1-22.el9.3.alma.2.x86_64.rpm">nginx-mod-devel-1.20.1-22.el9.3.alma.2.x86_64.rpm</a>  04-Mar-2026 11:22              761271
<a href="nginx-mod-http-image-filter-1.20.1-22.el9.3.alma.2.x86_64.rpm">nginx-mod-http-image-filter-1.20.1-22.el9.3.alm..&gt;</a> 04-Mar-2026 11:22               20013
<a href="nginx-mod-http-perl-1.20.1-22.el9.3.alma.2.x86_64.rpm">nginx-mod-http-perl-1.20.1-22.el9.3.alma.2.x86_..&gt;</a> 04-Mar-2026 11:22               31525
<a href="nginx-mod-http-xslt-filter-1.20.1-22.el9.3.alma.2.x86_64.rpm">nginx-mod-http-xslt-filter-1.20.1-22.el9.3.alma..&gt;</a> 04-Mar-2026 11:22               18809
<a href="nginx-mod-mail-1.20.1-22.el9.3.alma.2.x86_64.rpm">nginx-mod-mail-1.20.1-22.el9.3.alma.2.x86_64.rpm</a>   04-Mar-2026 11:22               54405
<a href="nginx-mod-stream-1.20.1-22.el9.3.alma.2.x86_64.rpm">nginx-mod-stream-1.20.1-22.el9.3.alma.2.x86_64.rpm</a> 04-Mar-2026 11:22               81059
</pre><hr></body>
</html>


[konstantin@localhost html]$ sudo nano /etc/yum.repos.d/otus.repo

[konstantin@localhost html]$ yum repolist enabled | grep otus
otus                otus-linux


[konstantin@localhost repo]$ sudo wget https://repo.percona.com/yum/percona-release-latest.noarch.rpm

--2026-03-04 14:39:24--  https://repo.percona.com/yum/percona-release-latest.noarch.rpm
Resolving repo.percona.com (repo.percona.com)... 49.12.125.205
Connecting to repo.percona.com (repo.percona.com)|49.12.125.205|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 28532 (28K) [application/x-redhat-package-manager]
Saving to: ‘percona-release-latest.noarch.rpm’

percona-release-latest.n 100%[=================================>]  27.86K  --.-KB/s    in 0.05s

2026-03-04 14:39:24 (594 KB/s) - ‘percona-release-latest.noarch.rpm’ saved [28532/28532]


[konstantin@localhost repo]$ sudo yum install -y percona-release.noarch
otus-linux                                                         400 kB/s | 7.2 kB     00:00
Last metadata expiration check: 0:00:01 ago on Wed Mar  4 14:41:31 2026.
Dependencies resolved.
===================================================================================================
 Package                       Architecture         Version               Repository          Size
===================================================================================================
Installing:
 percona-release               noarch               1.0-32                otus                28 k

Transaction Summary
===================================================================================================
Install  1 Package

Total download size: 28 k
Installed size: 50 k
Downloading Packages:
percona-release-latest.noarch.rpm                                  4.7 MB/s |  28 kB     00:00
---------------------------------------------------------------------------------------------------
Total                                                              2.7 MB/s |  28 kB     00:00
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                           1/1
  Installing       : percona-release-1.0-32.noarch                                             1/1
  Running scriptlet: percona-release-1.0-32.noarch                                             1/1
* Enabling the Percona Release repository
<*> All done!
* Enabling the Percona Telemetry repository
<*> All done!
* Enabling the PMM2 Client repository
<*> All done!
The percona-release package now contains a percona-release script that can enable additional repositories for our newer products.

Note: currently there are no repositories that contain Percona products or distributions enabled. We recommend you to enable Percona Distribution repositories instead of individual product repositories, because with the Distribution you will get not only the database itself but also a set of other componets that will help you work with your database.

For example, to enable the Percona Distribution for MySQL 8.0 repository use:

  percona-release setup pdps8.0

Note: To avoid conflicts with older product versions, the percona-release setup command may disable our original repository for some products.

For more information, please visit:
  https://docs.percona.com/percona-software-repositories/percona-release.html


  Verifying        : percona-release-1.0-32.noarch                                             1/1

Installed:
  percona-release-1.0-32.noarch

Complete!

