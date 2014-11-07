$setup = <<SCRIPT

echo "Stopping and removing existing containers"
#stop and remove any existing containers
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)

echo "Building from Dockerfiles"
# Build containers from Dockerfiles
docker build -t longie/redis /var/local/poc/dockerRedis
docker build -t longie/node /var/local/poc/dockerNode

echo "Running & linking containers"
# Run and link the containers
docker run -d -P --name redis longie/redis
docker run -d -P -p 9191:9191 -p 8181:8181 --name node --link redis:redis longie/node

SCRIPT

# Commands required to ensure correct docker containers
# are started when the vm is rebooted.
$start = <<SCRIPT
docker start redis
docker start node
SCRIPT

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure("2") do |config|

# Require a recent version of vagrant otherwise some have reported errors setting host names on boxes
Vagrant.require_version ">= 1.6.5"
    
    # Expose Redis database settings via NodeJS app
    config.vm.network "forwarded_port", guest: 9191, host: 9191

	 # Virtualbox customization
	 config.vm.provider :virtualbox do |virtualbox, override|
	
	   # Customize VM
	   virtualbox.customize ["modifyvm", :id, "--memory", "1024", "--cpus", "1", "--pae", "on", "--hwvirtex", "on", "--ioapic", "on", "--name", "SAP-Docker"]
	
	end
  
    # Ubuntu
    config.vm.box = "precise64"
    config.vm.box_url="http://files.vagrantup.com/precise64.box"

    # Install latest docker
    config.vm.provision "docker"

    config.vm.synced_folder ".", "/var/local/poc" #, type: "nfs"

    # Setup the containers, with caching enabled these will rebuild any changes since last build
    config.vm.provision "shell", run: "always", inline: $setup

    # Make sure the correct containers are running
    # every time we start the VM.
    config.vm.provision "shell", run: "always", inline: $start

end
