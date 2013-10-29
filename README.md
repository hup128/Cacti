Cacti
=====

VisTool setup instructions on RHEL6.4, aws instance
Instructions follows:

Ref. http://www.cyberciti.biz/faq/fedora-rhel-install-cacti-monitoring-rrd-software/
note bene.
62 mg file installation, logged in as root. (su sudo. Summary:

Installed:
  httpd.x86_64 0:2.2.15-29.el6_4                  mysql.x86_64 0:5.1.69-1.el6_4
  mysql-server.x86_64 0:5.1.69-1.el6_4            php.x86_64 0:5.3.3-23.el6_4
  php-cli.x86_64 0:5.3.3-23.el6_4                 php-common.x86_64 0:5.3.3-23.el6_4
  php-devel.x86_64 0:5.3.3-23.el6_4               php-gd.x86_64 0:5.3.3-23.el6_4
  php-mbstring.x86_64 0:5.3.3-23.el6_4            php-mysql.x86_64 0:5.3.3-23.el6_4
  php-pear.noarch 1:1.9.4-4.el6                   php-snmp.x86_64 0:5.3.3-23.el6_4

Dependency Installed:
  apr.x86_64 0:1.3.9-5.el6_2                     apr-util.x86_64 0:1.3.9-3.el6_0.1
  apr-util-ldap.x86_64 0:1.3.9-3.el6_0.1         autoconf.noarch 0:2.63-5.1.el6
  automake.noarch 0:1.11.1-4.el6                 httpd-tools.x86_64 0:2.2.15-29.el6_4
  libXpm.x86_64 0:3.5.10-2.el6                   lm_sensors-libs.x86_64 0:3.1.1-17.el6
  net-snmp.x86_64 1:5.5-44.el6_4.4               net-snmp-libs.x86_64 1:5.5-44.el6_4.4
  perl-DBD-MySQL.x86_64 0:4.013-3.el6            php-pdo.x86_64 0:5.3.3-23.el6_4

first start the mysql, create the mysql.sock in the var dir:
 sudo etc/rc.d/init.d/mysqld start
 And then:
 mysqladmin -u root password automata
 Then create Cacti mySQL database 
 
 Install snmpd. Summary:
 Installed:
  net-snmp-utils.x86_64 1:5.5-44.el6_4.4

Complete!

Set up the snmpd and make sure you get IP-MIB output

Install Cacti:
check if repo is active:
yum repolist

Log. :
Loaded plugins: amazon-id, rhui-lb, security
repo id                                   repo name                                      status
rhui-REGION-client-config-server-6        Red Hat Update Infrastructure 2.0 Client Confi      5
rhui-REGION-rhel-server-releases          Red Hat Enterprise Linux Server 6 (RPMs)       11,047
rhui-REGION-rhel-server-releases-optional Red Hat Enterprise Linux Server 6 Optional (RP  6,287

Still, cacti is not found as a package,
sol 1. Enable EPEL for RHEL 6.x / CentOS 6.x Users
Ref. http://www.cyberciti.biz/faq/rhel-fedora-centos-linux-enable-epel-repo/

Log.
[root@ip-10-141-143-7 /]# rpm -Uvh http://download.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm
Retrieving http://download.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm
warning: /var/tmp/rpm-tmp.7k6Jol: Header V3 RSA/SHA256 Signature, key ID 0608b895: NOKEY
Preparing...                ########################################### [100%]
   1:epel-release           ########################################### [100%]

Also, do the protect the Base Package:
# yum install yum-plugin-protectbase.noarch

Now running again the repolist:

[root@ip-10-141-143-7 /]# yum repolist
Loaded plugins: amazon-id, protectbase, rhui-lb, security
0 packages excluded due to repository protections
repo id                                              repo name                                                                  status
epel                                                 Extra Packages for Enterprise Linux 6 - x86_64                              9,847
rhui-REGION-client-config-server-6                   Red Hat Update Infrastructure 2.0 Client Configuration Server 6                 5
rhui-REGION-rhel-server-releases                     Red Hat Enterprise Linux Server 6 (RPMs)                                   11,047
rhui-REGION-rhel-server-releases-optional            Red Hat Enterprise Linux Server 6 Optional (RPMs)                           6,287
repolist: 27,186
[root@ip-10-141-143-7 /]#

Now yum install cacti works! (~9 M)

Install Log.
[root@ip-10-141-143-7 /]# yum install cacti
Loaded plugins: amazon-id, protectbase, rhui-lb, security
0 packages excluded due to repository protections
Setting up Install Process
Resolving Dependencies
--> Running transaction check
---> Package cacti.noarch 0:0.8.8b-3.el6 will be installed
--> Processing Dependency: rrdtool for package: cacti-0.8.8b-3.el6.noarch
--> Running transaction check
---> Package rrdtool.x86_64 0:1.3.8-6.el6 will be installed
--> Processing Dependency: dejavu-sans-mono-fonts for package: rrdtool-1.3.8-6.el6.x86_64
--> Processing Dependency: dejavu-lgc-sans-mono-fonts for package: rrdtool-1.3.8-6.el6.x86_64
--> Running transaction check
---> Package dejavu-lgc-sans-mono-fonts.noarch 0:2.30-2.el6 will be installed
---> Package dejavu-sans-mono-fonts.noarch 0:2.30-2.el6 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

======================================================================================================================================
 Package                               Arch              Version                    Repository                                   Size
======================================================================================================================================
Installing:
 cacti                                 noarch            0.8.8b-3.el6               epel                                        2.2 M
Installing for dependencies:
 dejavu-lgc-sans-mono-fonts            noarch            2.30-2.el6                 rhui-REGION-rhel-server-releases            392 k
 dejavu-sans-mono-fonts                noarch            2.30-2.el6                 rhui-REGION-rhel-server-releases            450 k
 rrdtool                               x86_64            1.3.8-6.el6                rhui-REGION-rhel-server-releases            294 k

Transaction Summary
======================================================================================================================================
Install       4 Package(s)

Total download size: 3.3 M
Installed size: 8.9 M
Is this ok [y/N]: y
Downloading Packages:
(1/4): cacti-0.8.8b-3.el6.noarch.rpm                                                                           | 2.2 MB     00:00
(2/4): dejavu-lgc-sans-mono-fonts-2.30-2.el6.noarch.rpm                                                        | 392 kB     00:00
(3/4): dejavu-sans-mono-fonts-2.30-2.el6.noarch.rpm                                                            | 450 kB     00:00
(4/4): rrdtool-1.3.8-6.el6.x86_64.rpm                                                                          | 294 kB     00:00
--------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                 5.0 MB/s | 3.3 MB     00:00
warning: rpmts_HdrFromFdno: Header V3 RSA/SHA256 Signature, key ID 0608b895: NOKEY
Retrieving key from file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-6
Importing GPG key 0x0608B895:
 Userid : EPEL (6) <epel@fedoraproject.org>
 Package: epel-release-6-8.noarch (installed)
 From   : /etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-6
Is this ok [y/N]: y
Running rpm_check_debug
Running Transaction Test
Transaction Test Succeeded
Running Transaction
  Installing : dejavu-lgc-sans-mono-fonts-2.30-2.el6.noarch                                                                       1/4
  Installing : dejavu-sans-mono-fonts-2.30-2.el6.noarch                                                                           2/4
  Installing : rrdtool-1.3.8-6.el6.x86_64                                                                                         3/4
  Installing : cacti-0.8.8b-3.el6.noarch                                                                                          4/4
  Verifying  : rrdtool-1.3.8-6.el6.x86_64                                                                                         1/4
  Verifying  : dejavu-sans-mono-fonts-2.30-2.el6.noarch                                                                           2/4
  Verifying  : dejavu-lgc-sans-mono-fonts-2.30-2.el6.noarch                                                                       3/4
  Verifying  : cacti-0.8.8b-3.el6.noarch                                                                                          4/4

Installed:
  cacti.noarch 0:0.8.8b-3.el6

Dependency Installed:
  dejavu-lgc-sans-mono-fonts.noarch 0:2.30-2.el6     dejavu-sans-mono-fonts.noarch 0:2.30-2.el6     rrdtool.x86_64 0:1.3.8-6.el6

Complete!
[root@ip-10-141-143-7 /]#





============================================================================================================

Problem loading Cacti on the browser.

The latest version of cacti (cacti-0.8.7d) requires that you edit include/config.php with the MySql details.
db.php might be the problem:
check:
 /var/www/cacti/include/config.php
 
 Try editing the file /etc/httpd/conf.d/cacti.conf.

Edit the line “allow from”
 






