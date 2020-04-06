# frozen_string_literal: true

Vagrant.configure('2') do |config|
  config.ssh.forward_agent = true

  config.vm.define :master_node, primary: true do |master_node|
    master_node.vm.hostname = 'k8smaster.local'
    master_node.vm.box = 'ubuntu/bionic64'
    master_node.vm.network :private_network, ip: '192.168.50.4'
    master_node.vm.provider :virtualbox do |virtualbox|
      virtualbox.name = 'k8smaster'
      virtualbox.gui = false
      virtualbox.memory = 2024
      virtualbox.cpus = 2
    end
  end

  config.vm.define :worker_node_1 do |worker_node_1|
    worker_node_1.vm.hostname = 'k8sworker1.local'
    worker_node_1.vm.box = 'ubuntu/bionic64'
    worker_node_1.vm.network :private_network, ip: '192.168.50.5'
    worker_node_1.vm.provider :virtualbox do |virtualbox|
      virtualbox.name = 'k8sworker1'
      virtualbox.gui = false
      virtualbox.memory = 2024
      virtualbox.cpus = 2
    end
  end

  config.vm.define :worker_node_2 do |worker_node_2|
    worker_node_2.vm.hostname = 'k8sworker2.local'
    worker_node_2.vm.box = 'ubuntu/bionic64'
    worker_node_2.vm.network :private_network, ip: '192.168.50.6'
    worker_node_2.vm.provider :virtualbox do |virtualbox|
      virtualbox.name = 'k8sworker2'
      virtualbox.gui = false
      virtualbox.memory = 2024
      virtualbox.cpus = 2
    end
  end
end
