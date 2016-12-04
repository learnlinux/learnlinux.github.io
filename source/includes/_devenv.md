
# Development Installation

To make development and testing easier, we’ve developed a tool to simulate TuxLab’s infrastructure on your local machine.  To do this, we use the Vagrant framework, a programmatic wrapper to the popular VirtualBox Virtual Machine. This provides one platform that can be used across all machines, allowing TuxLab’s architecture to work the same way regardless of your operating system or environment.

In order to set up TuxLab using vagrant, you must first install several frameworks and libraries on your local machine. These are:

 * VirtualBox (netadp, netflt, guest-iso)
 * Vagrant
 * Vagrant Guests Plugin
 * Ansible (Version 2.3 or above)
 * SSHPass
 
<aside class="notice">
BEFORE YOU PROCEED: You may enconter problems with ansible if you have a newer version of ruby installed on your machine. Version 2.2.4 should work best well. If you have ruby installed, you should roll back (or update) to this version using `sudo gem update --system 2.2.4`
</aside>

The following set of instructions will walk you through installation on our currently supported platforms.

## Red Hat / CentOS

### Before You Begin
It is recommended that you ensure you are running the latest kernel version of your OS. To check your kernel version, you should run

`uname -r`

Which should return something in the format:

`2.6.32-431.11.2.el6.x86_64`

To make sure this is the latest version, you can check it against the list of updated kernel versions found <a href="https://access.redhat.com/articles/3078">here</a>. You should also make sure you are the sudo user with the command `sudo -i`.

Finally, you should update your package list with the command `yum update`.

### Installing VirtualBox
Before installing virtualBox, you need to pull the virtualBox repository for RHEL and CentOS, by going into your yum repos directory and downloading it:

`cd /etc/yum.repos.d/`<br>
`wget http://download.virtualbox.org/virtualbox/rpm/rhel/virtualbox.repo`


Once this is done, you need to install the right dependency packages for virtualbox. The dependencies are bundled together for convenience. However, the bundle you need to use depends on the OS version you are running.

For RHEL/CoreOS version 7, run

`rpm -Uvh http://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-8.noarch.rpm`

For version 6, run

`rpm -Uvh http://dl.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm`

And for version 5, run

`rpm -Uvh http://dl.fedoraproject.org/pub/epel/5/i386/epel-release-5-4.noarch.rpm`

It does not appear that versions 4 or older are supported, so if you have an earlier version, you might have to work through a few things on your own. 

To finish installing dependencies, run

`yum install binutils gcc make patch libgomp glibc-headers glibc-devel kernel-headers kernel-devel dkms`

<aside class="notice">
Note: If you are using the PAE kernel, replace kernel-devel with kernel-PAE-devel.
</aside>

Now, you’re ready to install virtualBox via
`yum install VirtualBox-5.1`

### Installing Vagrant

You should be able to download and install vagrant itself rather easily, by running

`cd ~`<br>
`wget https://releases.hashicorp.com/vagrant/1.8.1/vagrant_1.8.1_x86_64.rpm`<br>
`sudo yum install vagrant_1.8.1_x86_64.rpm`<br>


Once this is done, try running the command `vagrant`. If vagrant is properly installed, you should see information about how to use vagrant and available subcommands. You will need to use those subcommands to add plugins in order to proceed. To do this, run the command

`vagrant plugin install vagrant-vbguest`

If this installs correctly you should see a message similar to

`Installed the plugin 'vagrant-vbguest (X.X.X)'!`
		
### Installing Ansible

Ansible can be downloaded using your package manager without downloading any additional package lists. Just run

`sudo yum install ansible`

### Installing SSHPass
To install SSHPass, you will need to download and build from the source code. The source can be found <a href="https://sourceforge.net/projects/sshpass/">here</a>. Once you download the source from this site, extract the files and cd into the newly extracted directory. Then run

`./configure`<br>
`sudo make install`


You’re now ready to clone our repo and get started.

### Setting Up TuxLab
Tuxlab’s infrastructure code is stored in a github repository. You don’t need to be very familiar with git to get the source, however. Just open a terminal, go to the directory you want to put the code in, and type 

`git clone https://github.com/learnlinux/tuxlab-infra.git`

Once this is done, cd into the newly created `tuxlab-infra` directory. Then run the command

`vagrant up`

If you’ve installed all the prerequisite programs properly, this should automatically run tuxlab’s infrastructure. The process may take any amount of time from 15 minutes or so to over an hour, so it is recommended you have a power source and a strong internet connection when you run this. 

Congratulations! You are now running tuxlab’s infrastructure in a development environment!

## Fedora

### Before You Begin

<aside class="notice">
Note: It is recommended that you Fedora version 22 or later. It should be possible with earlier versions, but you may encounter some unexpected problems with vagrant.
</aside>

It is recommended that you ensure you are running the latest kernel version of your OS. To check your kernel version, you should run

`uname -r`

To make sure this is the latest version, you can check it against the list of updated kernel versions found <a href="https://en.wikipedia.org/wiki/Fedora_version_history#Version_history">here</a>. You should also make sure you are the sudo user with the command `sudo -i`.
Findally, update your package repository using

`dnf update`

For versions 22 and above, or

`yum update`

For versions 21 and earlier.

### Installing VirtualBox

Before installing virtualBox, you need to pull the virtualBox repository for RHEL and CentOS, by going into your yum repos directory and downloading it:

`cd /etc/yum.repos.d/`<br>
`wget http://download.virtualbox.org/virtualbox/rpm/fedora/virtualbox.repo`

Once this is done, you need to install the right dependency packages for virtualbox. You can do this using

`dnf install binutils gcc make patch libgomp glibc-headers glibc-devel kernel-headers kernel-devel dkms`

for versions 22 or later, or 

`yum install binutils gcc make patch libgomp glibc-headers glibc-devel kernel-headers kernel-devel dkms`

for versions 21 or earlier.

<aside class="notice">
Note: If you are using the PAE kernel, replace kernel-devel with kernel-PAE-devel.
<\aside>

Now, you’re ready to install virtualBox via

`yum install VirtualBox-5.1`

### Installing Vagrant

For versions 22 or later, vagrant can be installed via

`dnf install vagrant`

If you’re using an earlier version of fedora, use

`yum install vagrant`

Once this is done, try running the command `vagrant`. If vagrant is properly installed, you should see information about how to use vagrant and available subcommands. You will need to use those subcommands to add plugins in order to proceed. To do this, run the command

`vagrant plugin install vagrant-vbguest`

If this installs correctly you should see a message similar to

`Installed the plugin 'vagrant-vbguest (X.X.X)'!`

### Installing Ansible

Ansible can be installed with your package manager with 

`dnf install ansible`

Or if you’re using version 21 or older, with

`yum install ansible`

### Installing SSHPass

To install SSHPass, you will need to download and build from the source code. The source can be found <a href="https://sourceforge.net/projects/sshpass/">here</a>. Once you download the source from this site, extract the files and cd into the newly extracted directory. Then run

`./configure`<br>
`sudo make install`

You’re now ready to clone our repo and get started.

### Setting Up TuxLab

Tuxlab’s infrastructure code is stored in a github repository. You don’t need to be very familiar with git to get the source, however. Just open a terminal, go to the directory you want to put the code in, and type 

`git clone https://github.com/learnlinux/tuxlab-infra.git`

Once this is done, cd into the newly created `tuxlab-infra` directory. Then, enter the command

`vagrant up`

If you’ve installed all the prerequisite programs properly, this should automatically run tuxlab’s infrastructure. The process may take any amount of time from 15 minutes or so to over an hour, so it is recommended you have a power source and a strong internet connection when you run this. 

Congratulations! You are now running tuxlab’s infrastructure in a development environment!

## Ubuntu / Debian

### Before you Begin

In order to ensure the installation process for your prerequisite programs goes smoothly, you should first make sure your package lists and installed programs are all up-to-date.  Run the following in Bash:

<aside class="notice">
	Note: running this second command will update all packages on your machine. This may include things like web browsers, so just keep in mind that running this second command may update more programs than are absolutely necessary. If you want to avoid this, it should be okay to omit the command.
</aside>

`sudo apt-get update`<br>
`sudo apt-get upgrade`


### Installing VirtualBox

Luckily, you can install virtualbox via Ubuntu’s package manager with the command:

`sudo apt-get install virtualbox`

### Installing Vagrant

Just like virtualbox, vagrant can be installed with your package manager with

`sudo apt-get install vagrant`

Once this is done, try running the command `vagrant`. If vagrant is properly installed, you should see information about how to use vagrant and available subcommands. You will need to use those subcommands to add plugins in order to proceed. To do this, run the command

`vagrant plugin install vagrant-vbguest`

If this installs correctly you should see a message similar to

`Installed the plugin 'vagrant-vbguest (X.X.X)'!`

### Installing Ansible
Though ansible may not be available right away via `apt-get`, ansible maintains a ppa which you can add in order to be able to install ansible using your package manager. The code to add the ppa and install ansible is as follows:

`sudo apt-get install software-properties-common`<br>
`sudo apt-add-repository ppa:ansible/ansible`<br>
`sudo apt-get update`<br>
`sudo apt-get install ansible`


### Installing SSHPass

SSHPass, like vagrant and virtualbox, can be installed by ubuntu’s package manager. Just run the command

`sudo apt-get install sshpass`

You’re now ready to clone our repo and get started.

### Setting Up TuxLab
Tuxlab’s infrastructure code is stored in a github repository. You don’t need to be very familiar with git to get the source, however. Just open a terminal, go to the directory you want to put the code in, and type 

`git clone https://github.com/learnlinux/tuxlab-infra.git`

Once this is done, cd into the newly created `tuxlab-infra` directory. Then run the command

`vagrant up`

If you’ve installed all the prerequisite programs properly, this should automatically run tuxlab’s infrastructure. The process may take any amount of time from 15 minutes or so to over an hour, so it is recommended you have a power source and a strong internet connection when you run this. 

Congratulations! You are now running tuxlab’s infrastructure in a development environment!

## Arch Linux / Manjaro

### Before You Begin

The best way to ensure a smooth installation is to ensure that your kernel is up-to-date, so if you haven’t updated in awhile, you should do so. As long as you are using an updated kernel, installation on archlinux should be very easy.

### Installing Prerequesites

Virtualbox, Vagrant, Ansible, and SSHPass can all be installed using pacman. Just run

`sudo pacman -S virtualbox`<br>
`sudo pacman -S vagrant`<br>
`sudo pacman -S ansible`<br>
`sudo pacman -S sshpass`


### Installing Vagrant Plugins

Once this is done, try running the command `vagrant`. If vagrant is properly installed, you should see information about how to use vagrant and available subcommands. You will need to use those subcommands to add plugins in order to proceed. To do this, run the command

`vagrant plugin install vagrant-vbguest`

If this installs correctly you should see a message similar to

`Installed the plugin 'vagrant-vbguest (X.X.X)'!`

You’re now ready to clone our repo and get started.

### Setting Up TuxLab

Tuxlab’s infrastructure code is stored in a github repository. You don’t need to be very familiar with git to get the source, however. Just open a terminal, go to the directory you want to put the code in, and type 

`git clone https://github.com/learnlinux/tuxlab-infra.git`

Once this is done, cd into the newly created `tuxlab-infra` directory. Then run the command

`vagrant up`

If you’ve installed all the prerequisite programs properly, this should automatically run tuxlab’s infrastructure. The process may take any amount of time from 15 minutes or so to over an hour, so it is recommended you have a power source and a strong internet connection when you run this. 

Congratulations! You are now running tuxlab’s infrastructure in a development environment!

## OS X

### Before You Begin

<aside class="notice">
WARNING: Many different libraries TuxLab uses do not support OS X. It is therefore not recommended that you use it.
</aside>

Make sure you have xcode installed by running

`sudo xcode-select --install`

### Installing VirtualBox

VirtualBox for OS X ships in a dmg file, which can be found <a href="https://www.virtualbox.org/wiki/Downloads">here</a>. To mount this, locate the .dmg file, then run the command

`hdiutil attach /path/to/VirtualBox-xyz.dmg`

Once the file is mounted, open a terminal and run

`sudo installer -pkg /Volumes/VirtualBox/VirtualBox.pkg -target /Volumes/Macintosh\ HD`

This should install virtualBox on your machine.

### Installing Vagrant

Vagrant for OS X ships in a dmg file, which can be found <a href="https://www.vagrantup.com/downloads.html">here</a>. Select the appropriate file to download. Once downloaded, you can install it simply by locating the file, and dragging it to your applications folder.

### Installing Ansible
	Installing ansible on OS X can be a bit tricky. We reccomend using <a href="http://binarynature.blogspot.com/2016/01/install-ansible-on-os-x-el-capitan_30.html">this installation guide</a>.
	
Once this is done, try running the command `vagrant`. If vagrant is properly installed, you should see information about how to use vagrant and available subcommands. You will need to use those subcommands to add plugins to vagrant in order to proceed. To do this, run the command

`vagrant plugin install vagrant-vbguest`

If this installs correctly you should see a message similar to

`Installed the plugin 'vagrant-vbguest (X.X.X)'!`

### SSHPass

To install SSHPass, you will need to download and build from the source code. The source can be found <a href="https://sourceforge.net/projects/sshpass/">here</a>. Once you download the source from this site, extract the files and cd into the newly extracted directory. Then run

`./configure`<br>
`sudo make install`

You’re now ready to clone our repo and get started.

### Setting Up TuxLab

Tuxlab’s infrastructure code is stored in a github repository. You don’t need to be very familiar with git to get the source, however. Just open a terminal, go to the directory you want to put the code in, and type 

`git clone https://github.com/learnlinux/tuxlab-infra.git`

Once this is done, cd into the newly created `tuxlab-infra` directory. Then, enter the command

`vagrant up`

If you’ve installed all the prerequisite programs properly, this should automatically run tuxlab’s infrastructure. The process may take any amount of time from 15 minutes or so to over an hour, so it is recommended you have a power source and a strong internet connection when you run this. 

Congratulations! You are now running tuxlab’s infrastructure in a development environment!
