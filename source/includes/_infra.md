# Getting Started
TuxLab is a complex web application, relying on several discrete servers to provide end service.  We will outline these services briefly, to make the operation of TuxLab more clear:

![TuxLab Infrastructure Diagram](https://docs.google.com/drawings/d/1jLnkbWYxgBlfEEc6eldGdA_ONhBRTjJ6KmwGvpoFXkY/pub?w=960&h=720)

### Ansible Tower
Ansible Tower is a popular platform for automating server provisioning and management.  The TuxLab AWS installer automatically provisions an Ansible Tower AMI Instance from AWS; so there is little to worry about from the management or operations perspective. However, Ansible Tower can be an invaluable resource in maintaining your TuxLab installation. We highly recommend you take a look at the Ansible and Ansible Tower documentation:

http://docs.ansible.com/ansible/tower.html  

### MongoDB
TuxLab relies on an external MongoDB Database to store user data, labs, and grades. This is not automatically setup by the TuxLab installer, because there are simply too many possible configurations to consider.  If you are looking for a test installation, we have found MongoLabs’ free hosted option to be a solid choice, however, it is not redundant, nor speedy enough to handle the requests for the application as a whole.  Instead, you should create your own MongoDB instance within AWS, using CloudForms to automate the scaling process.

In considering the size of your MongoDB cluster, it is important to consider that TuxLab relies on the Meteor Framework, which relies heavily on direct database reads from MongoDB, which we will discuss further in the next sub-section.

### Meteor
Meteor is a full-stack Javascript environment, hopefully simplifying development and operations by combining client and server-side operations.  When users first request the TuxLab site, they are given a large (>2MB) HTML and JS file built using Angular2, which contains all of the application assets.  This massively speeds up the processing of subsequent requests, as the only data that is communicated between client and server are the updates to a given page.

### Docker Swarm
To create virtual environments, TuxLab utilizes the popular Docker container framework.  A single instance, known as the Docker Master, runs the Docker warm Daemon, which connects to a number of other instances known as Docker Nodes.  At the command of the master, the nodes can create lab environments.

Users connect to these environments through a two-step process.  First, their computer requests DNS information for the host node, which is served by a Docker-based DNS Server on the Docker Master. The request response contains the IP address of one of the Docker Nodes, which your local machine uses to connect to a separate proxy container on the node.  The proxy forwards traffic from the public IP to the internal IP of the users’ container based on SSH username, allowing people to connect to their container from both the web and the command line.
