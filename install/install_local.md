# Local Installation

### Dependencies

#### Vagrant / Virtualbox
TuxLab uses Vagrant to provision virtual machines for TuxLab's service layer.
Vagrant in turn requires Virtualbox for virtualization.  This installation process
varies greatly from system-to-system, and is well documented here:

* https://www.virtualbox.org/wiki/Downloads
* https://www.vagrantup.com/downloads.html

Once Vagrant is installed, you need to install a number of plugins.  This
can be done with the following command:

```bash
  vagrant plugin install vagrant-hosts vagrant-hostsupdater vagrant-vbguest
```

#### Ansible
TuxLab uses Ansible to provision the systems (both physical and virtual) for
running TuxLab services.  Ansible can be installed through many native package
managers, but we instead recommend you follow the __PIP Installation__ guide
from the Ansible team:

* http://docs.ansible.com/ansible/latest/intro_installation.html#latest-releases-via-pip

### Creating the Virtual Environment

Download the latest version of the TuxLab Infrastructure from GitHub using the following command:

```bash
git clone --recursive https://github.com/learnlinux/tuxlab-infra.git
```

Then, from inside the TuxLab directory, create the virtual environment by
running the following command:

```
make up
```

With the virtual environment running, run the following command to start
the TuxLab server:
```
make run
```
This process can take a few minutes (especially at the first run), as by default
it compiles TuxLab from source.

### Troubleshooting

If you are having problems running TuxLab, log an issue at [our issue tracker](https://github.com/learnlinux/tuxlab-app/issues),
or reach out to us on [Gitter](https://gitter.im/learnlinux/tuxlab-app)
