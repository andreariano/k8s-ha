IMAGE_NAME = "bento/ubuntu-19.04"
NODES = 1

Vagrant.configure("2") do |config|
  # Box Settings
  config.vm.box = IMAGE_NAME

  # Provider Settings
  config.vm.provider "virtualbox" do |h|
    h.memory = 2048
    h.cpus = 2
  end

  config.vm.provision "shell", inline: "which python || sudo apt -y install python"

  config.vm.define "haproxy" do |lb|
    # VM basic setup
    lb.vm.hostname = "haproxy"
    lb.vm.network "private_network", ip: "192.168.50.10"
  end

  config.vm.define "k8s-master" do |master|
    # VM basic setup
    master.vm.hostname = "k8s-master"
    master.vm.network "private_network", ip: "192.168.50.11"
  end

  (1..NODES).each do |i|
    config.vm.define "k8s-node-#{i}" do |node|
      node.vm.hostname = "node-#{i}"
      node.vm.network "private_network", ip: "192.168.50.#{i + 11}"
    end
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "./ansible/k8s-ha.yaml"
    ansible.inventory_path = "./ansible/inventory/"
  end
end
