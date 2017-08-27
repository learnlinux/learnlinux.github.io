# Production Installation

---
<div class="note">
  <h3> TuxLab Sponsored Deployment </h3>

  Thanks to our generous sponsors at Red Hat, we are excited to
  provide hosted TuxLab instances for eligible Universities and
  K-12 Institutions.  If you are interested, fill out our application
  form:

  <br>
  <a class="material-button" href="https://docs.google.com/forms/d/e/1FAIpQLScrO1V3MTpE8qHgV5sESuQ2XDSCsqSf_IGpCOYzS_cZxanWog/viewform?usp=sf_link">APPLICATION FORM</a>
</div>

---

### System Requirements
TuxLab is comprised of a number of services, which are designed to
run on separate virtual instances:

#### MongoDB
TuxLab uses MongoDB as a database backend.  For small installations, this
can be co-located with the Meteor server.  However, in production we recommend
using a redundant MongoDB Cluster to ensure data integrity.  There are number of
ways to deploy MongoDB clusters to the cloud:

* [MongoDB Atlas Managed](https://www.mongodb.com/cloud/atlas/)
* [MLabs Managed](https://mlab.com/welcome/)
* [AWS Self-Managed](http://docs.aws.amazon.com/quickstart/latest/mongodb/architecture.html)
* [Bare Metal Self-Managed](https://github.com/twoyao/ansible-mongodb-cluster)

#### Meteor
[Meteor](https://www.meteor.com/) is an application server, integrating
Node.JS, MongoDB and Ansible. The Meteor application server is automatically
configured by the TuxLab Ansible Playbook, so all that is needed is a CentOS
7.X host.  We recommend using the following images:

* [ISO](https://www.centos.org/download/)
* [AWS](https://aws.amazon.com/marketplace/pp/B00O7WM7QW)
* [Vagrant](https://app.vagrantup.com/centos/boxes/7)

#### Docker Swarm Manager
TuxLab uses [Docker Swarm](https://docs.docker.com/swarm/) for scaling the Docker container execution environment
across servers.  The Docker Swarm Manager is the cluster manager for these systems,
as well as running several services including DNS.  It is automatically configured
by the TuxLab Ansible Playbook, so all that is needed is a CentOS Atomic host.

!> The version requirements for this system are specific (due to ETCD versioning), so
be sure to use the following images:

* [ISO](https://seven.centos.org/2016/10/new-centos-atomic-host-with-optional-docker-1-12/)
* [AWS](https://seven.centos.org/2016/10/new-centos-atomic-host-with-optional-docker-1-12/)
* [Vagrant](https://app.vagrantup.com/centos/boxes/atomic-host/versions/7.20161006)

#### Docker Swarm Host
TuxLab uses [Docker Swarm](https://docs.docker.com/swarm/) for scaling the Docker Container execution environment
across servers.  The Docker Swarm hosts are members of the Docker cluster, running the
lab runtime as well as proxy containers for providing access to labs.  It is automatically configured
by the TuxLab Ansible Playbook, so all that is needed is a CentOS Atomic host.


!> The version requirements for this system are specific (due to ETCD versioning), so
be sure to use the following images:

* [ISO](https://seven.centos.org/2016/10/new-centos-atomic-host-with-optional-docker-1-12/)
* [AWS](https://seven.centos.org/2016/10/new-centos-atomic-host-with-optional-docker-1-12/)
* [Vagrant](https://app.vagrantup.com/centos/boxes/atomic-host/versions/7.20161006)

### Download TuxLab Installer
#### Clone TuxLab-Infra
Clone the [tuxlab-infra](https://github.com/learnlinux/tuxlab-infra) repository
into a local directory.  Make sure to do it recursively, as it has submodule
dependencies:

```bash
git clone --recursive https://github.com/learnlinux/tuxlab-infra.git
```

#### Configuring TuxLab
Edit the `config.yml` configuration file found in the repository root.
The options are outlined here:

###### Domain Configuration

| Option          | Value                                                 |
|-----------------|-------------------------------------------------------|
| TUX_DOMAIN      | Domain name used for TuxLab                           |
| TUX_ORG         | Organization name, used in SSL CA.                    |
| TUX_EMAIL       | Service E-Mail.  Used in SSL CA.                   	  |
| TUX_HOMEPAGE    | Root URL.  Mut be suffixed by `/`                   	|

###### SSL Configuration

| Option          | Value                                                 |
|-----------------|-------------------------------------------------------|
| SSL_COUNTRY     | Organization country, used in SSL CA.                 |
| SSL_STATE       | Organization state, used in SSL CA.                   |
| SSL_LOCALITY    | Organization locality, used in SSL CA.              	|
| SSL_ORG         | Organization name, used in SSL CA.                   	|
| SSL_ORG_UNIT    | Organization unit name, used in SSL CA.               |

###### LabEnv Configuration

| Option          | Value                                                                     |
|-----------------|---------------------------------------------------------------------------|
| LAB_IMAGES      | A list of Lab Images to be downloaded from the Docker Hub.  [Read more]() |

##### Auth Configuration
###### Google Authentication

| Option                     | Value                                                |
|----------------------------|------------------------------------------------------|
| AUTH_GOOGLE                | Whether to use Google Authentication                 |
| AUTH_GOOGLE_CLIENTID       | Google API Client ID                                 |
| AUTH_GOOGLE_SECRET         | Google API Secret ID              	                  |
| AUTH_GOOGLE_DOMAIN         | Limit to a single email domain.  (Optional)          |

###### SAML Authentication
Not yet implemented.  A work in progress.

### Provisioning using Ansible
Once the requisite systems are configured, create an Ansible inventory file
containing the systems you wish to provision. For example:

```yaml
\[all:vars\]
swarm_node_ip: 10.100.1.10

\[tuxlab-swarm-manager\]
dswarm ansible_host=10.100.1.10 ansible_port=22 ansible_user='username' ansible_password='password'

\[tuxlab-swarm-host\]
dhost ansible_host=10.100.1.11 ansible_port=22 ansible_user='username' ansible_password='password'

\[tuxlab-meteor\]
meteor ansible_host=10.100.1.2 ansible_port=22 ansible_user='username' ansible_password='password'
```

Then execute the
