# Deployment

Instructions for the FogCore system deployment.



To test FogCore system deployment you need to have 2 hosts running Ubuntu 20.04 with SSH server and know their IP addresses.

#### First host

1. Connect to a remote host via ssh

   ```bash
   ssh <username>@<host_ip>
   ```

2. Clone Deployment repository

   ```bash
   git clone https://github.com/FogCore/Deployment.git && cd Deployment
   ```

3. Install Docker Engine

   ```bash
   ./docker_install.sh
   ```

4. Create the necessary container volumes

   ```bash
   docker volume create --name=CloudletsServiceDB && docker volume create --name=ImagesServiceDB && docker volume create --name=ImagesServiceRegistry && docker volume create --name=UsersServiceDB && docker volume create --name=SchedulingServiceDB
   ```

5. Prepare the **docker-compose.yml** file

   1. In the **services** -> **SchedulingService** -> **environment** -> **DOCKER_SWARM_MANAGER** field set the IP address of the **second** host
   2. In the **services** -> **SchedulingService** -> **environment** -> **DOCKER_REGISTRY_URL** field set the IP address of **this** host

6. Prepare the **docker_registry_config.yml** file

   1. In the **notifications** -> **endpoints** -> **url** field set the IP address of **this** host
   2. In the **auth** -> **token** -> **realm** field set the IP address of **this** host

7. Start FogCore

   ```bash
   docker-compose up
   ```



#### Second host

1. Connect to a remote host via ssh

   ```bash
   ssh <username>@<host_ip>
   ```

2. Clone Deployment repository

   ```bash
   git clone https://github.com/FogCore/Deployment.git && cd Deployment
   ```

3. Start installing the required components

   During the installation, you need to specify the IP address or domain name of the Docker Registry (see ImagesService) in the following format `ip_address`:`port`.

   ```bash
   sudo ./docker_swarm_manager_install.sh
   ```

4. Add a user to the docker group

   ```bash
   sudo usermod -aG docker $USER && newgrp - docker
   ```



To add fog nodes, see the instructions in the [CloudletAgent](https://github.com/FogCore/CloudletAgent) repository.


**The reported project was supported by RFBR, research project No. 18-07-01224**
