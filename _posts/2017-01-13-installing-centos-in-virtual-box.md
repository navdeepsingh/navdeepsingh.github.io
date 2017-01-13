---
layout: post
date:   2017-01-13 11:23:00 +0530
author:     "Navdeep Singh"
header-img : "img/centos-in-virtualbox.jpg"
categories: virtualbox
---
Installing CentOS in Virtual Box of Windows Host Machine for Development
========================================================================

<p>It was really excited to use Linux based OS in my Windows Host Machine through Oracle based Virtual Box Manager. Since many months i want to use Linux in Windows. So at last when i assigned with task to fix some bugs in Drupal site that is installed in CentOS with httpd and mariadb and I need to replicate server and site on my local to fix bugs then understands this all stuff altogether. The need to create replica first because we cant do directly change the css on server Drupal site and second it got some direct changes on server and we cant take risk to break the live site by discarding the direct live changes on server.</p>

1.Installing CentOS

![image-title-here](/img/note1.png){:class="img-responsive"}
First installed CentOS-7 from iso file which downloaded from centOS website.

2. Installing VBoxGuestAdditions.iso and setting up

Actually this step is essential to mount host machine development folder in virtual host machine.  This VBoxAdditions allows your VM to setup shared folders and other good things. We'll need shared folders to allow your host machine to edit files and have your guest machine use those updated files. Once that is done, you'll need to restart your VM and remove the VBoxAdditions ISO CD.

First select VBoxGuestAdditions.iso in storage device and select folder to mount. And following commands need to run.
{% highlight js %}
su root
mkdir /media/cdrom
mount -r /dev/cdrom /media/cdrom
yum -y update kernel*
yum group install "Development Tools"
sudo /media/cdrom/VBoxLinuxAdditions.run
{% endhighlight %}

Reference links : [http://toolkt.com/site/virtualbox-shared-folders-with-centos-server-guest/](http://toolkt.com/site/virtualbox-shared-folders-with-centos-server-guest/)
[http://www.pc-freak.net/blog/installing-virtualbox-guest-additions-vboxadditions-centos-65-fedora-19-20-rhel-65-510/](http://www.pc-freak.net/blog/installing-virtualbox-guest-additions-vboxadditions-centos-65-fedora-19-20-rhel-65-510/)

![image-title-here](/img/note2.png){:class="img-responsive"}

By default the path is created and symbolic link is created for easy reference.
{% highlight js %}
/media/sf_www
ln -s /media/sf_www /www
{% endhighlight %}

Edit the file /etc/sysconfig/selinux to disable it. Change the string from SELINUX=enforcing to SELINUX=disabled. Save it and exit.
{% highlight js %}
vi /etc/sysconfig/selinux
{% endhighlight %}

![image-title-here](/img/note3.png){:class="img-responsive"}

Now SELINUX is disabled. We now need to fix our networking. To do that we need to edit our network file at this location :/etc/sysconfig/network-scripts/ifcfg-enp0s3
{% highlight js %}
vi /etc/sysconfig/network-scripts/ifcfg-enp0s3
{% endhighlight %}

![image-title-here](/img/note4.png){:class="img-responsive"}

{% highlight js %}
service network restart
{% endhighlight %}

Now we have network setup

3. Install NGINX and MariaDB

Below reference links are best to install nginx and mariadb
[https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-on-centos-7](https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-on-centos-7)
[https://www.nginx.com/resources/wiki/start/topics/tutorials/install/](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/)

In precise following commands used to install same
{% highlight js %}
sudo yum install epel-release
sudo yum install nginx
sudo systemctl start nginx
{% endhighlight %}

![image-title-here](/img/note5.png){:class="img-responsive"}

The way to find ip address of virtual machine
{% highlight js %}
ip addr
{% endhighlight %}

![image-title-here](/img/note6.png){:class="img-responsive"}

Install MariaDB
{% highlight js %}
sudo yum install mariadb-server mariadb
sudo systemctl start mariadb
sudo mysql_secure_installation
sudo systemctl enable mariadb
{% endhighlight %}

Login to MariaDB
{% highlight js %}
mysql -u root -p
{% endhighlight %}

Reference Link [https://www.linode.com/docs/databases/mariadb/how-to-install-mariadb-on-centos-7](https://www.linode.com/docs/databases/mariadb/how-to-install-mariadb-on-centos-7)

Install PHP
{% highlight js %}
sudo yum install php php-mysql php-fpm
sudo vi /etc/php.ini
cgi.fix_pathinfo=0
sudo vi /etc/php-fpm.d/www.conf
listen = /var/run/php-fpm/php-fpm.sock
listen.owner = nobody
listen.group = nobody
user = nginx
group = nginx
sudo systemctl start php-fpm
{% endhighlight %}

Working in Vim Editor
{% highlight js %}
# Open Edit file
vi filename.php

# Edit Mode
Press Insert Key

# Save & Quit
Press Escape Key + :wq + Enter

# Quit without saving
Press :q! + Enter

# Quit
Press :q + Enter

// Cut, Copy, Paste
# Press v to select characters
# Press V to select whole line

# Press d to cut
# Press y to cut

# Press p to paste
{% endhighlight %}

Hope to continue my development in LEMP environment with nodejs powered apps. Future is Javascript seems so.
