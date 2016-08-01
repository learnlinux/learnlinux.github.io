# Installation
## Development Environment
In order to make development and evaluation easier, we have created a Vagrant Environment to simulate the servers needed to run the TuxLab Site. You can get this up and running by first installing the following pre-requisites:
 * Vagrant
 * VirtualBox (netadp, netflt, guest-iso)
 * Vagrant Guests Plugin
 * VirtualBox
 * Ansible (>2.2)
 * SSHPass

You can then initialize this environment by running `make vagrant`. You will also need to add `10.100.0.10` in your [HOSTS file](http://www.howtogeek.com/howto/27350/beginner-geek-how-to-edit-your-hosts-file/).

## AWS Cloud
You need the following things:
 * Install Python, pip and Ansible (=>2.2) via your package manager.
 * Install boto and tower-cli using pip (`sudo pip install -U boto ansible-tower-cli`)
 * Place your Ansible Tower license in /aws/keys/tower.txt, and add the JSON property `"eula_accepted" : true`, indicating you have read and accepted the Ansible Tower EULA.
 * Edit the `aws/vars/` files based on your configuration.
 * Set the AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY Enviornment Variables, and run the ansible playbook:
    ```
    export AWS_ACCESS_KEY_ID=XXXXXXXXXXXXXX
    export AWS_SECRET_ACCESS_KEY=XXXXXXXXXXXXXXXXXXXX
    ansible-playbook site.yml
    ```
 * Finally, create DNS records to route your domain to the TuxLab instance. If you wanted for the tuxlab app to be displayed at domain.com.
   ```
   NS   *.domain.com     <SWARM_NODE_IP>
   A    domain.com       <LOAD_BALANCER_DNS_NAME>
   ```
   Alternatively, If you are using Amazon Route 53, you can use [Aliases](http://docs.aws.amazon.com/ElasticLoadBalancing/latest/DeveloperGuide/using-domain-names-with-elb.html#dns-associate-custom-elb) to more easily configure these IP addresses.