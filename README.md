# rediscluster
Ansible to deploy redis cluster in 6 docker containers (3 masters and 3 slaves) 

Note: Please don't use Vagrant to deploy the VMs, issue will be resolved in next version 
## Requirements

- VM must not share the same physical host as they use the same ports and ports are hard coded.  

## Getting Started

This deployement script use the role ansible-role-docker created by [githubixx](https://github.com/githubixx) in order to install Docker components on the Nodes.

To run redis cluster :

  1. Download the repo or use git to clone in your home directory: git clone https://github.com/ghassencherni/mysqlcluster.git
  2. Add the entry the IPs of your nodes (node1, node2, node3) in the hosts file ( /etc/hosts ) 
  5. Move to the downloaded repo : cd mysqlcluster
  6. Run the command: "ansible-playbook deploy-redis-cluster.yml -i hosts" 
