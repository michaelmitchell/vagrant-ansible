# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
	# box config
	config.vm.box = "jskoolaid/ubuntu-14.04"
	config.vm.network "private_network", ip: "192.168.33.10"
	config.vm.hostname = "vagrant-ansible"

	# folder sync config
	#config.vm.synced_folder ".", "/var/www", :mount_options => ["dmode=777", "fmode=666"]
	config.vm.synced_folder ".", "/var/www", :nfs => {:mount_options => ["dmode=777","fmode=666"]}

	# hostmanager config
	config.hostmanager.enabled = true
	config.hostmanager.manage_host = true
	config.hostmanager.ignore_private_ip = false
	config.hostmanager.include_offline = true
	config.hostmanager.aliases = %w(vagrant-ansible vagrant-ansible.local vagrant-ansible.dev)

	# aws provider config
	config.vm.provider :aws do |aws, override|
		# aws access details, uses environment variables
		aws.access_key_id = ENV["AWS_ACCESS_KEY"] # export AWS_ACCESS_KEY=aws_access_key
		aws.secret_access_key  = ENV["AWS_SECRET_KEY"] # export AWS_SECRET_KEY=aws_secret_key

		# configure instance type and region
		aws.instance_type = "t2.micro"
		aws.region = "ap-southeast-1"

		# use box name by default for keypair and default security group
		aws.keypair_name = config.vm.box
		aws.security_groups = [config.vm.box]

		# configure ssh access, private key should be from keypair matching boxname
		override.ssh.username = "ubuntu"
		override.ssh.private_key_path = "/home/michael/.ssh/jskoolaid_ubuntu-1404.pem"
		override.nfs.functional = false
	end

	# virtualbox provider config
	config.vm.provider "docker" do |d|
		d.email = ENV["DOCKER_EMAIL"]
		d.username = ENV["DOCKER_USERNAME"]
		d.password = ENV["DOCKER_PASSWORD"]
		d.auth_server = "https://index.docker.io/v1/"
	end

	# virtualbox provider config
	config.vm.provider "virtualbox" do |v|
		v.memory = 512
		v.cpus = 2
	end

	# vmware provider config
	config.vm.provider "vmware_desktop" do |v|
	  v.vmx["memsize"] = "512"
	  v.vmx["numvcpus"] = "2"
	end

	config.vm.provision :ansible do |ansible|
		ansible.playbook = "ansible/main.yml"
	end
end
