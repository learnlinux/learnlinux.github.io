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


## Arch Linux / Manjaro
