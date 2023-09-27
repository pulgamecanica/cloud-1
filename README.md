# cloud-1
By Felipe & André
***

## About the project

The main objective of the project is to develop an automatic deployment using an automator tool.
The website to be deployed is the Inception project from the Common Core.
We decided to use ansible to deploy our Wordpress Site.

***

## Mandaroty Tasks

- [ ] Your site can restart automatically if the server is rebooted. ⏯️
- [ ] In case of reboot all the data of the site are persisted (images, user accounts, articles, ...). 🧰
- [ ] It is possible to deploy your site on several servers in parallel. 🔃
- [ ] The script must be able to function in an automated way with for only assumption an ubuntu 20.04 LTS like OS of the target instance running an SSH daemon and with Python installed.
- [X] Your applications will run in separate containers that can communicate with each other (1 process = 1 container)
- [ ] Public access to your server must be limited and secure (for example, it is not possible to connect directly to your database from the internet).
- [X] The services will be the different components of a WordPress to install by yourself. For example Phpmyadmin, MySQL, ...
- [X] You must have a docker-compose.yml.
- [X] You will need to ensure that your SQL database works with WordPress and PHPMyAdmin
- [X] Your server should be able, when possible, to use TLS
- [ ] You will need to make sure that, depending on the URL requested, your server redirects to the correct site.

***

## Ideas

	To restart automatically we should explore Ansible and how deployment works with their playbooks.
	Ensure we have a volume which is stored somewhere in the cloud (s3, or RDS) this way, the data and the site information persists.
	To serve the website in parallel we should implement a Target Group with several EC2 instances serving the website, read AWS docs.

 	Our script should function in an automated way, no hard coding.
  	The application should run separate containers which can communicate with each other, this is achieved with Inception already.
   	The access to the server is limited and secure, the EC2 should only open access to port 22 and 443, keep the DB port closed to the internet. No connection is possible, but locally.
	The services will be different components of Wordpress, which Inception already does, using docker containers, and a docker-compose.yml.
 	The SQL database (MariaDB) does work with PHPMMyAdmin, you can test by visiting url/wp-admin.
  	The configuration will always force the use of TLS when possible.
   	The target group should redirect to the right servers always, even on restart of any EC2 instance. The site should redirect correctly to every page.

***


## EC2 Amazon

#### EC2

- Login in your AWS account
- Create a new Ec2 instance (I used all the defaults for ubuntu)
- Setup the security context, open ports 22 and 444 for HTTP/S requests
- Create a key-pair (downloaded the .pem file)
- Launched instance
- Got Instance ssh information
- Connected to my EC2:
	``` ssh -i cloud1-1.pem ubuntu@ec2-?-?-?-?.eu-west-2.compute.amazonaws.com ```
- Installed my repo: which contains inception
- cd to the repo & make all
- docker ps (make sure the docker containers are running properly)
- Visit your EC2 IP

#### Route53

- Obtain your Domain Name (DN)
- Setup the Hosted Zone -> Add a DNS Record to the Hosted Zone (type A for IPv4), input the EC2 IP
- Visit the DN make sure the redirection is done correctly

***

#### EC2 Docker installation

```sh
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update # [FAILED]
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin # [FAILED]
sudo snap install docker # [WORKED]
docker --version

```

***

#### EC2 Make installation

```sh
sudo apt install make

```

***

### Docker Initialization

```bash
make

docker compose -f srcs/docker-compose.yml up --build -d
permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/containers/json?all=1&filters=%7B%22label%22%3A%7B%22com.docker.compose.config-hash%22%3Atrue%2C%22com.docker.compose.project%3Dsrcs%22%3Atrue%7D%7D": dial unix /var/run/docker.sock: connect: permission denied
```

***

##### Failed, user doesn't have permissions to use docker engine.


	Post-Docker Instalation

```sh
# https://docs.docker.com/engine/install/linux-postinstall/
# Create docker group
sudo groupadd docker
# Add User to docker group
sudo usermod -aG docker $USER
# Commit changes to group
newgrp docker
```

	Reboot EC2 instance & it should work now :)


> Make & Verify

```sh
curl https://www.arosado-.42.fr # Need Certificate
curl http://www.arosado-.42.fr # Forbiden, not secure

```

***

#### Access the NGINX server through the internet

