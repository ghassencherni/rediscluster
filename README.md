# rediscluster
Ansible to deploy redis cluster in 6 docker containers (3 masters and 3 slaves) 

## Requirements

- 4 VMs : Ansible controler + 3 nodes: in my case i have used an Ansible on my laptop ( ubuntu 18.04) and three VMs on MS Azure (Centos 7) 
- Ports 6001 and 6002 ( deafault ports ) must be opened in the VMs firewall

## Getting Started

This deployement script use the role ansible-role-docker created by [githubixx](https://github.com/githubixx) in order to install Docker components on the Nodes.
*If you get some issues with the docker installation using the ansible-role-docker, please tro install it manually. 

To run redis cluster :

  1. Download the repo or use git to clone in your home directory: git clone https://github.com/ghassencherni/rediscluster.git
  2. Add the entry the IPs of your 3 nodes (node1, node2, node3) in the hosts file ( /etc/hosts ) 
  5. Move to the downloaded repo : cd rediscluster
  6. Run the command: "ansible-playbook deploy-redis-cluster.yml -i hosts" 

## How to test the deploy :

   1. Connect to any redis docker on the 3 nodes 
   2- run the command :
   redis-cli -p 6001 ( for the container 1 , redis-1) or redis-cli -p 6002 ( for the redis-2) 
   3- check the replication status:
   redis-cli > info replication
   
 Note) for more information about redis cluster please check redis.io
 
 ## Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):


Adding new Bridge network configuration and use it with the cluster due to an issue with the default bridge 
  
     bridge_name: redis_bridge
     
     bridge_subnet: 172.18.10.0/16

     bridge_gateway: 172.18.10.1
-------------------------------------------

Container 1 configuration 

     container_1_name: redis-1

     redis_1_port: 6001                   # The normal Redis TCP port used to serve clients

     redis_1_data_port: 16001             # Adding 10000 to the Redis port


Container 2 configuration

    container_2_name: redis-2

    redis_2_data_port: 16002

    redis_2_port: 6002


Containers IP must be in bridge subnet

    container_1_ip: 172.18.10.2

    container_2_ip: 172.18.10.3

IP node: Used to create the redis cluster, IPs can be different from IP hosts used by ansible ( example: ansible uses public IP, redis cluster nodes uses private IPs to communicate )

     node1_ip: 10.0.0.4

     node2_ip: 10.0.0.5

     node3_ip: 10.0.0.6

