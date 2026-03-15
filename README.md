# Setting-Up-SSH-Connection-Between-Two-Ubuntu-Containers-in-Docker
This project demonstrates how to establish a secure remote connection (SSH) between two separate Ubuntu containers (server-client) running inside Docker
Scenario Overview
This scenario demonstrates how to establish a secure remote connection (SSH) between two separate Ubuntu containers running inside Docker.
Normally, containers communicate using application protocols (HTTP, APIs) but in this project we simulate a real server-client environment using SSH to understand:

Container networking.
Secure remote access.
How containers behave like isolated systems with their own network stack.
This setup is useful when:
Practicing DevOps and system administration.
Testing automation tools (Ansible, scripts, CI/CD).
Simulating multiple Linux servers for testing.
Getting Started
- Pull Ubuntu Image
docker pull ubuntu
Press enter or click to view image in full size

Create the first Docker container
docker run -it --name container1 ubuntu // -it to get interactive shell inside it
update the packages

apt-get update
Press enter or click to view image in full size

- Install openssh server
apt-get install openssh-server
Configure SSH on the first container
we need to edit SSH configuration file using the following command

nano /etc/ssh/sshd_config
uncomment the line and change it to yes

Press enter or click to view image in full size

Press enter or click to view image in full size

This tells the SSH server: “Allow root login with password.”

Now check SSH status

Press enter or click to view image in full size

Create the second container
docker run -it --name container2 ubuntu
Update the package list

apt-get update
in this container we’ll install openssh client

apt-get install openssh-client
Press enter or click to view image in full size

Now let’s switch to the container1:

docker exec -it container1 bash
check the password using the following command

cat /etc/shadow | grep root 
The command will display the root user’s password hash. Since relying on the hash itself isn’t practical, you should set a new password for the root account using the following command:

Press enter or click to view image in full size

Now from the second container we have to connect to the first container which is SSH Server, let’s check the IP address before:

docker inspect container1 | grep IPAddress
Press enter or click to view image in full size

Now switch to container2 ( SSH Client )

docker exec -it container2 bash
Connect to the ssh server by running the following command

ssh root@172.17.0.2
Press enter or click to view image in full size

We’ve now connected from the client container to the server container
