# Docker-Guacamole

## Requirements

#### CentOS

1. Install Docker CE

	- Install the required packages

		```# yum install -y yum-utils device-mapper-persistent-data lvm2```

	- Add the stable repository

		```# yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo```

	- Install Docker CE and containerd

		```# yum install docker-ce docker-ce-cli containerd.io```

	- Start and enable Docker
	
		```# systemctl start docker```
		
		```# systemctl enable docker```

2. Install Docker Compose

	- Check the [current release](https://github.com/docker/compose/releases) and replace the version in URL for the command below.

		```# curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose```

	- Mark the Docker Compose binary as executable

		```# chmod +x /usr/local/bin/docker-compose```

- For more detail, see the [Docker documentation](https://docs.docker.com/install/linux/docker-ce/centos/) on the Docker website

#### Debian

1. Install Docker CE

	- Install the required packages

		```# apt install apt-transport-https ca-certificates curl gnupg2 software-properties-common```

	- Add Docker's official GPG 
		```# curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add -```

	- Add the stable repository

		```# sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux debian $(lsb_release -cs) stable"```

	- Install Docker CE and containerd

		```# apt update```

		```# apt install docker-ce docker-ce-cli containerd.io```

	- Start and enable Docker
	
		```# systemctl start docker```
		
		```# systemctl enable docker```

2. Install Docker Compose

	- Check the [current release](https://github.com/docker/compose/releases) and replace the version in URL for the command below.

		```# curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose```

	- Mark the Docker Compose binary as executable

		```# chmod +x /usr/local/bin/docker-compose```

- For more detail, see the [Docker documentation](https://docs.docker.com/install/linux/docker-ce/debian/) on the Docker website

## Install

1.  Initialize the MySQL Database

	```# docker run --rm guacamole/guacamole /opt/guacamole/bin/initdb.sh --mysql > initdb.sql```

2. Edit the .env file
  * Be sure to at least change the default passwords

3. Build the Dockerfile

	```# docker-compose build```

4. Start the services

	```# docker-compose up -d```


## Upgrade

1. Copy SQL update script from the Guacamole container to the host (Note: replace `*.*.*` with the version you are upgrading to)

	```# docker cp guacamole_client_1:/opt/guacamole/mysql/schema/upgrade/upgrade-pre-*.*.*.sql ./```

2. Copy the SQL update script from the host to the MySQL container

	```# docker cp upgrade-pre-*.*.*.sql guacamole_database_1:/tmp```

3. Import the script into the database

	```# docker exec -it guacamole_database_1 bash```

	```# mysql -u root -p guacamole_db < /tmp/upgrade-pre-*.*.*.sql```

4. Edit the `docker-compose.yml` file to reflect the new version

5. Stop the containers and start the new ones

	```# docker-compose stop```

	```# docker-compose up -d```
