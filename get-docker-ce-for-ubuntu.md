# Get Docker CE for Ubuntu

To get started with Docker CE on Ubuntu, make sure you [meet the prerequisites](https://docs.docker.com/install/linux/docker-ce/ubuntu/#prerequisites), then [install Docker](https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-docker-ce).

### Prerequisites {#prerequisites}

#### Docker EE customers {#docker-ee-customers}

To install Docker Enterprise Edition \(Docker EE\), go to [Get Docker EE for Ubuntu](https://docs.docker.com/install/linux/docker-ee/ubuntu/) **instead of this topic**.

To learn more about Docker EE, see [Docker Enterprise Edition](https://www.docker.com/enterprise-edition/).

#### OS requirements {#os-requirements}

To install Docker CE, you need the 64-bit version of one of these Ubuntu versions:

* Bionic 18.04 \(LTS\)
* Artful 17.10
* Xenial 16.04 \(LTS\)
* Trusty 14.04 \(LTS\)

Docker CE is supported on Ubuntu on `x86_64`, `armhf`, `s390x` \(IBM Z\), and `ppc64le` \(IBM Power\) architectures.

> **`ppc64le` and `s390x` limitations**: Packages for IBM Z and Power architectures are only available on Ubuntu Xenial and above.

#### Uninstall old versions {#uninstall-old-versions}

Older versions of Docker were called `docker` or `docker-engine`. If these are installed, uninstall them:

```text
$ sudo apt-get remove docker docker-engine docker.io
```

It’s OK if `apt-get` reports that none of these packages are installed.

The contents of `/var/lib/docker/`, including images, containers, volumes, and networks, are preserved. The Docker CE package is now called `docker-ce`.

#### Supported storage drivers {#supported-storage-drivers}

Docker CE on Ubuntu supports `overlay2` and `aufs` storage drivers.

* For new installations on version 4 and higher of the Linux kernel, `overlay2` is supported and preferred over `aufs`.
* For version 3 of the Linux kernel, `aufs` is supported because `overlay` or `overlay2` drivers are not supported by that kernel version.

If you need to use `aufs`, you need to do additional preparation as outlined below.

**EXTRA STEPS FOR AUFS**

* Xenial 16.04 and newer
* Trusty 14.04

For Ubuntu 16.04 and higher, the Linux kernel includes support for OverlayFS, and Docker CE uses the `overlay2`storage driver by default. If you need to use `aufs` instead, you need to configure it manually. See [aufs](https://docs.docker.com/engine/userguide/storagedriver/aufs-driver/)

### Install Docker CE {#install-docker-ce}

You can install Docker CE in different ways, depending on your needs:

* Most users [set up Docker’s repositories](https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-using-the-repository) and install from them, for ease of installation and upgrade tasks. This is the recommended approach.
* Some users download the DEB package and [install it manually](https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-from-a-package) and manage upgrades completely manually. This is useful in situations such as installing Docker on air-gapped systems with no access to the internet.
* In testing and development environments, some users choose to use automated [convenience scripts](https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-using-the-convenience-script) to install Docker.

#### Install using the repository {#install-using-the-repository}

Before you install Docker CE for the first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker from the repository.

**SET UP THE REPOSITORY**

1. Update the `apt` package index:

   ```text
   $ sudo apt-get update
   ```

2. Install packages to allow `apt` to use a repository over HTTPS:

   ```text
   $ sudo apt-get install \
       apt-transport-https \
       ca-certificates \
       curl \
       software-properties-common
   ```

3. Add Docker’s official GPG key:

   ```text
   $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
   ```

   Verify that you now have the key with the fingerprint`9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88`, by searching for the last 8 characters of the fingerprint.

   ```text
   $ sudo apt-key fingerprint 0EBFCD88

   pub   4096R/0EBFCD88 2017-02-22
         Key fingerprint = 9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
   uid                  Docker Release (CE deb) <docker@docker.com>
   sub   4096R/F273FCD8 2017-02-22
   ```

4. Use the following command to set up the **stable** repository. You always need the **stable** repository, even if you want to install builds from the **edge** or **test** repositories as well. To add the **edge** or **test** repository, add the word `edge` or `test` \(or both\) after the word `stable` in the commands below.

   > **Note**: The `lsb_release -cs` sub-command below returns the name of your Ubuntu distribution, such as `xenial`. Sometimes, in a distribution like Linux Mint, you might need to change `$(lsb_release -cs)` to your parent Ubuntu distribution. For example, if you are using`Linux Mint Rafaela`, you could use `trusty`.

   * x86\_64 / amd64
   * armhf
   * IBM Power \(ppc64le\)
   * IBM Z \(s390x\)

   ```text
   $ sudo add-apt-repository \
      "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) \
      stable"
   ```

   > **Note**: Starting with Docker 17.06, stable releases are also pushed to the **edge** and **test** repositories.

   [Learn about **stable** and **edge** channels](https://docs.docker.com/install/).

**INSTALL DOCKER CE**

1. Update the `apt` package index.

   ```text
   $ sudo apt-get update
   ```

2. Install the _latest version_ of Docker CE, or go to the next step to install a specific version:

   ```text
   $ sudo apt-get install docker-ce
   ```

   > Got multiple Docker repositories?
   >
   > If you have multiple Docker repositories enabled, installing or updating without specifying a version in the `apt-get install` or `apt-get update` command always installs the highest possible version, which may not be appropriate for your stability needs.

3. To install a _specific version_ of Docker CE, list the available versions in the repo, then select and install:

   a. List the versions available in your repo:

   ```text
   $ apt-cache madison docker-ce

   docker-ce | 18.03.0~ce-0~ubuntu | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
   ```

   b. Install a specific version by its fully qualified package name, which is package name \(`docker-ce`\) “=” version string \(2nd column\), for example, `docker-ce=18.03.0~ce-0~ubuntu`.

   ```text
   $ sudo apt-get install docker-ce=<VERSION>
   ```

   The Docker daemon starts automatically.

4. Verify that Docker CE is installed correctly by running the `hello-world` image.

   ```text
   $ sudo docker run hello-world
   ```

   This command downloads a test image and runs it in a container. When the container runs, it prints an informational message and exits.

Docker CE is installed and running. The `docker` group is created but no users are added to it. You need to use `sudo` to run Docker commands. Continue to [Linux postinstall](https://docs.docker.com/install/linux/linux-postinstall/) to allow non-privileged users to run Docker commands and for other optional configuration steps.

**UPGRADE DOCKER CE**

To upgrade Docker CE, first run `sudo apt-get update`, then follow the [installation instructions](https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-docker), choosing the new version you want to install.

#### Install from a package {#install-from-a-package}

If you cannot use Docker’s repository to install Docker CE, you can download the `.deb` file for your release and install it manually. You need to download a new file each time you want to upgrade Docker CE.

1. Go to [https://download.docker.com/linux/ubuntu/dists/](https://download.docker.com/linux/ubuntu/dists/), choose your Ubuntu version, browse to `pool/stable/` and choose `amd64`, `armhf`, `ppc64el`, or `s390x`. Download the `.deb` file for the Docker version you want to install.

   > **Note**: To install an **edge** package, change the word `stable` in the URL to `edge`. [Learn about **stable**and **edge** channels](https://docs.docker.com/install/).

2. Install Docker CE, changing the path below to the path where you downloaded the Docker package.

   ```text
   $ sudo dpkg -i /path/to/package.deb
   ```

   The Docker daemon starts automatically.

3. Verify that Docker CE is installed correctly by running the `hello-world` image.

   ```text
   $ sudo docker run hello-world
   ```

   This command downloads a test image and runs it in a container. When the container runs, it prints an informational message and exits.

Docker CE is installed and running. The `docker` group is created but no users are added to it. You need to use `sudo` to run Docker commands. Continue to [Post-installation steps for Linux](https://docs.docker.com/install/linux/linux-postinstall/) to allow non-privileged users to run Docker commands and for other optional configuration steps.

**UPGRADE DOCKER CE**

To upgrade Docker CE, download the newer package file and repeat the [installation procedure](https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-from-a-package), pointing to the new file.

#### Install using the convenience script {#install-using-the-convenience-script}

Docker provides convenience scripts at [get.docker.com](https://get.docker.com/) and [test.docker.com](https://test.docker.com/) for installing edge and testing versions of Docker CE into development environments quickly and non-interactively. The source code for the scripts is in the[`docker-install` repository](https://github.com/docker/docker-install). **Using these scripts is not recommended for production environments**, and you should understand the potential risks before you use them:

* The scripts require `root` or `sudo` privileges to run. Therefore, you should carefully examine and audit the scripts before running them.
* The scripts attempt to detect your Linux distribution and version and configure your package management system for you. In addition, the scripts do not allow you to customize any installation parameters. This may lead to an unsupported configuration, either from Docker’s point of view or from your own organization’s guidelines and standards.
* The scripts install all dependencies and recommendations of the package manager without asking for confirmation. This may install a large number of packages, depending on the current configuration of your host machine.
* The script does not provide options to specify which version of Docker to install, and installs the latest version that is released in the “edge” channel.
* Do not use the convenience script if Docker has already been installed on the host machine using another mechanism.

This example uses the script at [get.docker.com](https://get.docker.com/) to install the latest release of Docker CE on Linux. To install the latest testing version, use [test.docker.com](https://test.docker.com/) instead. In each of the commands below, replace each occurrence of `get` with `test`.

> **Warning**:
>
> Always examine scripts downloaded from the internet before running them locally.

```text
$ curl -fsSL https://get.docker.com -o get-docker.sh
$ sudo sh get-docker.sh

<output truncated>

If you would like to use Docker as a non-root user, you should now consider
adding your user to the "docker" group with something like:

  sudo usermod -aG docker your-user

Remember to log out and back in for this to take effect!

WARNING: Adding a user to the "docker" group grants the ability to run
         containers which can be used to obtain root privileges on the
         docker host.
         Refer to https://docs.docker.com/engine/security/security/#docker-daemon-attack-surface
         for more information.
```

Docker CE is installed. It starts automatically on `DEB`-based distributions. On `RPM`-based distributions, you need to start it manually using the appropriate `systemctl` or `service` command. As the message indicates, non-root users can’t run Docker commands by default.

**UPGRADE DOCKER AFTER USING THE CONVENIENCE SCRIPT**

If you installed Docker using the convenience script, you should upgrade Docker using your package manager directly. There is no advantage to re-running the convenience script, and it can cause issues if it attempts to re-add repositories which have already been added to the host machine.

### Uninstall Docker CE {#uninstall-docker-ce}

1. Uninstall the Docker CE package:

   ```text
   $ sudo apt-get purge docker-ce
   ```

2. Images, containers, volumes, or customized configuration files on your host are not automatically removed. To delete all images, containers, and volumes:

   ```text
   $ sudo rm -rf /var/lib/docker
   ```

You must delete any edited configuration files manually.

### Next steps {#next-steps}

* Continue to [Post-installation steps for Linux](https://docs.docker.com/install/linux/linux-postinstall/)
* Continue with the [User Guide](https://docs.docker.com/engine/userguide/).

[requirements](https://docs.docker.com/glossary/?term=requirements), [apt](https://docs.docker.com/glossary/?term=apt), [installation](https://docs.docker.com/glossary/?term=installation), [ubuntu](https://docs.docker.com/glossary/?term=ubuntu), [install](https://docs.docker.com/glossary/?term=install), [uninstall](https://docs.docker.com/glossary/?term=uninstall), [upgrade](https://docs.docker.com/glossary/?term=upgrade), [update](https://docs.docker.com/glossary/?term=update)  


