# Development Installation

To make development and testing easier, we’ve developed a tool to simulate TuxLab’s infrastructure on your local machine.  To do this, we use the Vagrant framework, a programmatic wrapper to the popular VirtualBox Virtual Machine. This provides one platform that can be used across all machines, allowing TuxLab’s architecture to work the same way regardless of your operating system or environment.

In order to set up TuxLab using vagrant, you must first install several frameworks and libraries on your local machine. These are:

 * VirtualBox (netadp, netflt, guest-iso)
 * Vagrant
 * Vagrant Guests Plugin
 * Ansible (Version 2.3 or above)
 * SSHPass

 The following set of instructions will walk you through installation on our currently supported platforms.
## Red Hat / CentOS
## Fedora

## Ubuntu / Debian
### Before you Begin
In order to ensure the installation process for your prerequisite programs goes smoothly, you should first make sure your package lists and installed programs are all up-to-date.  Run the following in Bash:

`sudo apt-get update`<br>
`sudo apt-get upgrade`

<aside class="notice">
	Note: running this second command will update all packages on your machine. This may include things like web browsers, so just keep in mind that running this second command may update more programs than are absolutely necessary. If you want to avoid this, it should be okay to omit the command.
</aside>

### Installing VirtualBox
Luckily, you can install virtualbox via Ubuntu’s package manager with the command:
`sudo apt-get install virtualbox`
### Installing Vagrant
Just like virtualbox, vagrant can be installed with your package manager with 
sudo apt-get install vagrant
Once this is done, try running the command vagrant. If vagrant is properly installed, you should see information about how to use vagrant and available subcommands. You will need to use those subcommands to add plugins and virtual environments (“boxes”) to vagrant in order to proceed. First, you need to install the guests plugin for vagrant. To do this, run the command

`vagrant plugin install vagrant-vbguest`

If this installs correctly you should see a message similar to

`Installed the plugin 'vagrant-vbguest (X.X.X)'!`

Now, you need to add two “boxes” to vagrant: the centos box and the coreos/7 box. A box is essentially the kernel of a virtual environment, and TuxLab runs both centos and coreos/7 boxes. To install the boxes, run 

`vagrant box add centos/7 \ http://cloud.centos.org/centos/7/vagrant/x86_64/images/CentOS-7-x86_64-Vagrant-1610_01.VirtualBox.box -f`
`vagrant box add coreos-stable URLGOESHERE`
	YOU SHOULD SEE ______....
### Installing Ansible
Though ansible may not be available right away via apt-get, ansible maintains a ppa which you can add in order to be able to install ansible using your package manager. The code to add the ppa and install ansible is as follows:

```sudo apt-get install software-properties-common
sudo apt-add-repository ppa:ansible/ansible
sudo apt-get update
sudo apt-get install ansible```
(via http://docs.ansible.com/ansible/intro_installation.html#installation)
### SSHPass
SSHPass, like vagrant and virtualbox, can be installed by ubuntu’s package manager. Just run the command

`sudo apt-get install sshpass`

Congratulations! You’re ready to clone our repo and get started!


## Arch Linux / Manjaro
