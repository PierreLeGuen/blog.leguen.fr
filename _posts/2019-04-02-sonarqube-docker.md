# Setup SonarQube with docker for C project

## Installing docker

For Arch Linux users:

```bash
# Use the AUR to install docker
$ yaourt -S docker
```

Other distributions:

* Use the [official docker documentation](https://docs.docker.com/install/) 

## Setting up SonarQube in Docker

It's time to start docker on your computer.

```bash
# Start and enable docker
$ sudo systemctl enable docker
$ sudo systemctl start docker
```

Now you can pull the SonarQube docker from the official repo of sonarqube

```bash
# Use docker pull to get the container
$ sudo docker pull sonarqube
```

When the download is finished you can start your SonarQube image

```bash
# Let's start the container on port 9000
$ sudo docker run -d --name sonarqube -p 9000:9000 sonarqube
# Check if it's running
$ sudo docker container ls                                                                                     
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                    NAMES
032e450f3328        sonarqube           "./bin/run.sh"      9 seconds ago       Up 8 seconds        0.0.0.0:9000->9000/tcp   sonarqube
```

### Accessing SonarQube

We just started the docker container in localhost.

To access our SonarQube installation you just need to use your favorite web browser, and enter this address:

* http://localhost:9000/

The default username and password, are `admin` and `admin`.

### Installing Sonar Scanner

1. Go to [this SonarQube webpage](https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner) and download the Linux 64 bit version.
2. 