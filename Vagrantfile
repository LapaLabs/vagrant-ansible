# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"
#ENV["VAGRANT_DEFAULT_PROVIDER"] = "parallels" # The "parallels" or "virtualbox" providers are available

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Vagrant Hostmanager plugin
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.include_offline = true

  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.
  config.vm.hostname = "symfony.local"

  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "ubuntu/trusty64" # Use "virtualbox" box by default

  # Override default VirtualBox VM box with Parallels if Parallels provider is used
  config.vm.provider "parallels" do |p, override|
    override.vm.box = "parallels/ubuntu-14.04"
  end

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.66.66"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # If true, then any SSH connections made will enable agent forwarding.
  # Default value: false
  # config.ssh.forward_agent = true

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  config.vm.synced_folder "./application", "/var/www/html/application", type: "nfs"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for Parallels:
  config.vm.provider "parallels" do |p|
    p.name = "symfony.local"
    p.memory = 1024
    p.cpus = 1
    p.update_guest_tools = true # Allow auto-update plugin
    p.optimize_power_consumption = false # Override "Longer Battery Life" with "Better Performance"
  
    # To customize the VM. Mounted specified ISO image on its virtual media device (cdrom).
    #p.customize ["set", :id, "--device-set", "cdrom0", "--image", "/path/to/disk.iso", "--connect"]
  end
  # Example for VirtualBox:
  config.vm.provider "virtualbox" do |v|
    v.name = "symfony.local"
    v.memory = 1024
    v.cpus = 1
  
    # Use VBoxManage to customize the VM. For example to change cpuexecutioncap:
    v.customize ["modifyvm", :id, "--cpuexecutioncap", "100"]
  end

  config.vm.provision "ansible" do |a|
    a.playbook = "./ansible/provision.yml"
  end
end
