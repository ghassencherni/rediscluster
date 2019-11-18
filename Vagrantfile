VAGRANTFILE_API_VERSION = "2"
 Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
 config.vm.box = "genebean/centos-7-docker-ce"
 config.vm.hostname = "Node1"
 # Create a private network, which allows host-only access to the machine
 # using a specific IP.
 config.vm.network :private_network, ip: "10.0.0.2"
 # config.vm.network "forwarded_port", guest: 80, host: 8888
 # config.vm.network :public_network, :bridge => "eth1", :auto_config => false
 # Share an additional folder to the guest VM
 # config.vm.synced_folder ".", "/vagrant", disabled: true
 # config.vm.synced_folder ".", "/home/vagrant/provision", type: "rsync"
 config.vm.provider :virtualbox do |vb|
 # Set VM memory size
 vb.customize ["modifyvm", :id, "--memory", "1024"]
 # these 2 commands massively speed up DNS resolution, which means outbound
 # connections don't take forever (eg the WP admin dashboard and update page)
 vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
 vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
 end
 end
