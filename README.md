# cloud-1

***

## About the project

***

## Tesis

***

## Execution


***


## EC2 Amazon

- Login in your AWS account
- Create a new Ec2 instance (I used all the defaults for ubuntu)
- I created a key-pair (downloaded the .pem file)
- Launched instance
- Got Instance ssh information
- Connected to my EC2:
	``` ssh -i cloud1-1.pem ubuntu@ec2-?-?-?-?.eu-west-2.compute.amazonaws.com ```
- Installed my repo: which contains inception

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

