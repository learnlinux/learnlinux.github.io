# Production Installation
TuxLab uses Ansible to automate the installation process, which makes TuxLab completely infrastructure-agnostic.  However, setting up the application using your own infrastructure is an advanced procedure.  To simplify this, we have created a local Ansible script which automates the process of installing TuxLab on Amazon Web Services, and includes features like auto-scaling and provisioning.

## AWS
### Create an AWS Account
In order to provision instances on AWS, you will need to create an AWS Account.  Students and instructors are eligible to receive free credits for AWS via their University, if you are looking to try out TuxLab without an initial cost.  To register, simply [complete the form](https://aws.amazon.com/) on AWS’ website:
![Signup for AWS](images/aws/aws_signup.png)

<aside class="notice">
Academic users (Students, Faculty, Administrators) are eligible to receive credit from Amazon Web Services.  The easiest way to do this is using the [GitHub Student Pack](https://education.github.com/pack).
</aside>

### Obtain API Key
You will then need to obtain an API Key, allowing the TuxLab installer to provision infrastructure on your account.  In order to do this, navigate to the “My Security Credentials” dropdown:

![AWS Security Credentials](images/aws/aws_sec.png)

Then choose “create new access key”:

![AWS Keys](images/aws/aws_keys.png)

And finally, choose to download the key file.  We will use this later in running the install script.

![AWS Download Key](images/aws/aws_down.png)

## Ansible Tower
### Obtain License Key
In addition to the TuxLab server applications, the installer will install Ansible Tower, a tool used to automatically scale, provision and check the health of the TuxLab service.  Ansible Tower is an open source product, however it requires a license for updating, support, and ease of operation.  All users are entitled to a free Ansible Tower license for up to ten nodes (which is substantially larger than what is needed of most TuxLab installations). First, [complete the registration](https://www.ansible.com/license) online:

![Ansible Tower Keys](images/aws/tower_signup.png)

### Accept EULA
You will then be emailed a license key, which is simply a JSON string (you will also recieve an email containing a tarball.  Ignore this.).  You need to add a property; `"eula_accepted" : true`, to this file, confirming that you have read the End User License Agreement.  Save this file for later use.

## Setup Ansible
Install Ansible and the prerequisites as explained earlier in the Development Installation section.

In addition, you need to install the boto package, which is used for interfacing with AWS.  The easiest way to install this is using PIP, which should work on all platforms:
`pip install boto`.

## Setup MongoDB
TuxLab relies on an external installation of MongoDB to store application data.  There are a number of ways to setup MongoDB, some of which are outlined in our prior section on the Mongo component of TuxLab.  Before proceeding, it is important to make note of your MongoDB URL and authentication details, and insert them into the appropriate variable in the next step.

## Configuring TuxLab
Prior to running the installation script, it is important that you review and edit the configuration settings.  These settings are outlined in detail in the configuration section of the documentation.

<aside class="warning">
In setting up each server, Ansible automatically pulls the configuration details from the Git repository of each project (tuxlab-app, tuxlab-infra).  It is therefor essential that you fork these repositories, and edit the Git URL variables to use these repositories instead of the defaults.  This process is described in more depth in the Configuration documentation.
</aside>

## Installing TuxLab
Run the following command in the `tuxlab-infra` repository, substituting the AWS_ACCESS_KEY_ID and the AWS_SECRET_ACCESS_KEY with the values you downloaded previously in the AWS keyfile:

`export AWS_ACCESS_KEY_ID=XXXXXXXXXXXXXX`<br>
`export AWS_SECRET_ACCESS_KEY=XXXXXXXXXXXXXXXXXXXX`<br>
`ansible-playbook site.yml`

You will be prompted to enter an Ansible password, which you should take note of.  You will use it later to connect to the internet Ansible dashboard.

## Setup DNS
Finally, create DNS records to route your domain to the TuxLab instance. If you wanted for the tuxlab app to be displayed at domain.com, you would set the following records:
`  NS   *.domain.com     <SWARM_NODE_IP>`<br>
`  A      domain.com     <LOAD_BALANCER_DNS_NAME>`<br>
Alternatively, If you are using Amazon Route 53, you can use [Aliases](http://docs.aws.amazon.com/ElasticLoadBalancing/latest/DeveloperGuide/using-domain-names-with-elb.html#dns-associate-custom-elb) to more easily configure these IP addresses.

## Viewing Status of TuxLab Installation
Once the command has run to completion, you will see a number of instances active on your AWS account:


Locate the one labeled “Ansible_Tower,” and note its public DNS Address.  If you enter this into your browser, prefixed by https://, you will pull up the Ansible Tower dashboard, which you can use to check the status of your TuxLab installation.
