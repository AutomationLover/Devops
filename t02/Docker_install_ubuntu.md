
# How to Install Docker on Ubuntu

## Pre-requisites
Ensure that you have the following:
1. A Ubuntu system
2. sudo privileges

## Steps

### Update your existing list of packages
```bash
sudo apt update
```

### Install a few prerequisite packages which let `apt` use packages over HTTPS
```bash
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```

### Add the GPG key for the official Docker repository to your system
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

### Add the Docker repository to APT sources
```bash
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

### Update the package database with the Docker packages from the newly added repo
```bash
sudo apt update
```

### Make sure you are about to install from the Docker repo instead of the default Ubuntu repo
```bash
apt-cache policy docker-ce
```

### Finally, install Docker
```bash
sudo apt install docker-ce
```

### Docker should now be installed, the daemon started, and the process enabled to start on boot. Check that it's running
```bash
sudo systemctl status docker
```

## Congratulations! You have successfully installed Docker. 

For more official Docker documentation, visit their official [website](https://www.docker.com/) or their [official Docker documentation](https://docs.docker.com/).
