# LabVMs

### What is a LabVM Image?
TuxLab provisions [Docker Containers](https://www.docker.com/what-container) for
use by students in completing labs.  These containers are created using images
from the Docker Hub, however, only **specifically-designed** containers will
work with TuxLab.  We call these images **LabVM images**, as they can be used to
execute TuxLab Labs.

### Official LabVM Images
The TuxLab project currently publishes
two official LabVM images:

[**Alpine**](https://hub.docker.com/r/tuxlab/labvm-alpine/)
Alpine is a Linux distribuition designed for running on Docker containers, and
includes a simple to use package manager called `apk`.  Because Alpine is lightweight,
it is the best choice for most TuxLab usecases.

[**Red Hat Enterprise Linux**](https://hub.docker.com/r/tuxlab/labvm-rhel7/).
Red Hat Enterprise Linux (RHEL) is the most popular commercial Linux distribuition
and is trusted by IT professionals for use in production contexts.  RHEL includes
support for many features and tools not included in lightweight distributions (like Alpine),
and is thus is the best choice for courses targeted at more advanced users.

### Creating a LabVM Image
There are two reasons to create your own Lab VM Image:

#### 1) You require a specific operating system
Perhaps the most obvious reason to create a Lab VM is if you
want to use a specific operating system (besides Alpine and RHEL).
Almost all major distribuitions have an existing Docker Container image. However,
because TuxLab interacts with the operating system in a programmatic way,
Docker images intended for use with TuxLab need to be modified in a
few ways:

* A password should be randomly generated for the TuxLab user, and stored in `/pass`.
* SSHD should be installed and configured.
* An interactive shell (ie bash) should be installed and configured.

A good example of this is the [Alpine LabVM Dockerfile](https://github.com/learnlinux/tuxlab-labvm-alpine),
which is built on top of the lightweight Alpine container image.

#### 2) You want to speed setup of LabVMs
Each time a lab is executed by the user, the setup commands are executed
on a new Docker container provisioned with the designated LabVM.  This can
take a long time if the commands include slow tasks, like downloading
large or numerous packages, or running configuration steps.

Instead of performing this at execution time, you can create a new Dockerfile which
pre-installs and configures the lab environment.  For example, consider the following
LabVM Dockerfile which pre-installs `vim`:

```docker
FROM tuxlab/labvm-alpine
RUN apk add vim

```

>! Docker maintains copies of images as [a series of layers](https://docs.docker.com/engine/userguide/storagedriver/imagesandcontainers/),
so try to use similar base images for all the labs in an environment to conserve
storage space.

### Publishing a LabVM Image
Once you have created a new Dockerfile for your LabVM image, it must be published
on Docker Hub in order for TuxLab to access the image.  This can be done by
performing the steps in this guide:

* https://docs.docker.com/docker-cloud/builds

We recommend following the instructions specifically for setting up automated builds
from GitHub, as this encourages code transparency and eases the process of making changes
to your image later:

* https://docs.docker.com/docker-hub/github/#linking-docker-hub-to-a-github-account

### Configuring your LabVM inside a Labfile
In order to use your newly created LabVM inside a Labfile, you must define a
number of configuration options.  This is done inside the `new TuxLab()` block at
the top of the Labfile:

```javascript

Lab = new TuxLab({
  name: "Lab Name",
  description: "Lab Description",
  vm : {
    image: "tuxlab/labvm-alpine", // Image name on Docker Hub
    entry_cmd: ["./entry.sh"], // Docker Entryfile

    // Function which appends and escapes strings for execution by shell
    shell_fn: (cmd : string[]) => {
      return ['/bin/bash', '-c', cmd.join(" ").replace('"','\"')];
    },
    ssh_port: 22, // Exposed SSH Port. NOTE: this doesn't effect SSH connection port.
    username: "root", // Username
    password_path: "/pass" // Password Location
  }
})

```
