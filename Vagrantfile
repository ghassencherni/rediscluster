servers=[
  {
    :hostname => "Node4",
    :ip => "10.0.0.4",
    :box => "generic/centos7",
    :ram => 1024,
    :cpu => 1,
    :redis_1_fw => 6011,
    :redis_2_fw => 6012,
    :redis_data_1_fw => 16011,
    :redis_data_2_fw => 16012
  },
  {
    :hostname => "Node5",
    :ip => "10.0.0.5",
    :box => "generic/centos7",
    :ram => 1024,
    :cpu => 1,
    :redis_1_fw => 6021,
    :redis_2_fw => 6022,
    :redis_data_1_fw => 16021,
    :redis_data_2_fw => 16022
  },
  {
    :hostname => "Node6",
    :ip => "10.0.0.6",
    :box => "generic/centos7",
    :ram => 1024,
    :cpu => 1,
    :redis_1_fw => 6031,
    :redis_2_fw => 6032,
    :redis_data_1_fw => 16031,
    :redis_data_2_fw => 16032
  }
]
Vagrant.configure(2) do |config|
    servers.each do |machine|
        config.vm.define machine[:hostname] do |node|
            node.vm.box = machine[:box]
            node.vm.hostname = machine[:hostname]
            node.vm.network "private_network", ip: machine[:ip]
            node.vm.provider "virtualbox" do |vb|
                vb.customize ["modifyvm", :id, "--memory", machine[:ram]]
            end
            # Port forwording for container 1
            node.vm.network "forwarded_port", guest: machine[:redis_1_fw], host: 6001
            # Port forwording for container 2
            node.vm.network "forwarded_port", guest: machine[:redis_2_fw], host: 6002
            # Port forwording for container 1
            node.vm.network "forwarded_port", guest: machine[:redis_data_1_fw], host: 16001
            # Port forwording for container 2
            node.vm.network "forwarded_port", guest: machine[:redis_data_2_fw], host: 16002
            
            node.vm.provision "shell", inline: <<-SHELL
              sudo yum update
              yum -y install python2-pip
              sudo pip install --upgrade pip
              pip install docker-py
              sudo yum -y install docker

            #Open mysql master and slave ports
            firewall-cmd --zone=public --add-port=6001/tcp --permanent
            firewall-cmd --zone=public --add-port=6002/tcp --permanent
            firewall-cmd --zone=public --add-port=16001/tcp --permanent
            firewall-cmd --zone=public --add-port=16002/tcp --permanent
            firewall-cmd --reload
            SHELL
            node.vm.provision :ansible do |ansible|
               ansible.playbook = "deploy-redis-cluster.yml"
               ansible.inventory_path= "hosts"
               ansible.limit = machine[:hostname]
               ansible.sudo = true
               ansible.host_key_checking = false
            end
        end
    end
end

