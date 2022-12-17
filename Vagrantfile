# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV['VAGRANT_DEFAULT_PROVIDER'] = 'virtualbox'

# Check for required plugin(s)
['vagrant-hostmanager'].each do |plugin|
  unless Vagrant.has_plugin?(plugin)
    raise "#{plugin} plugin not found. Please install it via 'vagrant plugin install #{plugin}'"
  end
end


Vagrant.configure("2") do |config|

if Vagrant.has_plugin?('vagrant-hostmanager') then
  config.hostmanager.enabled = true
  config.hostmanager.manage_guest = true
  config.hostmanager.manage_host = false
end

if Vagrant.has_plugin?('vagrant-vbguest') then
  config.vbguest.auto_update = false
end

#config.vm.provider "hyperv" do |h, override|
#  h.enable_virtualization_extensions = true
#  h.linked_clone = true
#  h.maxmemory = 2048
#  h.cpus = 2

#  override.vm.network "public_network", bridge: "Default Switch"
#end

#config.vm.provider "vmware_desktop" do |h|
#  h.gui = false
#  h.vmx["memsize"] = "2048"
#  h.vmx["numvcpus"] = "2"
#end

#config.vm.provider "libvirt" do |h|
#  h.graphics_type = "none"
#  h.linked_clone = true
#  h.memory = 2048
#  h.cpus = 2
#end

config.vm.provider "virtualbox" do |h, override|
  h.gui = false
  h.memory = 2048
  h.cpus = 2

#  override.vm.network "private_network", type: "dhcp"
end

config.vm.synced_folder '.', '/vagrant', disabled: true
config.vm.box_check_update = false
config.ssh.forward_agent = true
config.ssh.insert_key = false
#config.ssh.extra_args = ["-o", "HostkeyAlgorithms=+ssh-rsa", "-o", "PubkeyAcceptedAlgorithms=+ssh-rsa"]

config.vm.define "ansible", primary: true do |ansible|
  ansible.vm.box = "generic/ubuntu2204"
  ansible.vm.hostname = "webserver"
  ansible.vm.network "private_network", ip: "192.168.56.10"

  ansible.vm.provision "file", source: "#{Dir.home}/.vagrant.d/insecure_private_key", destination: ".ssh/id_rsa"
  ansible.vm.provision "shell", privileged: false, inline: <<-SHELL
    chmod 600 ~vagrant/.ssh/id_rsa
  SHELL

  ansible.vm.provision "shell", inline: <<-SHELL
    apt update
    apt install -y python3-pip
  SHELL

  ansible.vm.provision "shell", privileged: false, inline: <<-SHELL
    python3 -m pip install --user ansible
  SHELL
end

#config.vm.define "centos" do |centos|
#  centos.vm.box = "generic/centos8"
#  centos.vm.hostname = "centos"
#  centos.vm.network "private_network", ip: "192.168.56.20"
#end

config.vm.define "ubuntu" do |ubuntu|
  ubuntu.vm.box = "generic/ubuntu2204"
  ubuntu.vm.hostname = "nameserver"
  ubuntu.vm.network "private_network", ip: "192.168.56.20"
#  gitea.vm.network "forwarded_port", guest: 3000, host: 3000, auto_correct: true
end

end
