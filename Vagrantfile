ubuntu_box = 'ubuntu/trusty64'

nodes = 4

Vagrant.configure(2) do |config|

  if Vagrant.has_plugin?('vagrant-hostmanager')
    config.hostmanager.enabled = true
    config.hostmanager.manage_host = true
  end

  if Vagrant.has_plugin?('vagrant-vbguest')
    config.vbguest.auto_update = false
  end

  1.upto(nodes) do |i|
    config.vm.define "ops#{i}" do |nodeconf|

      nodeconf.vm.hostname = "ops#{i}"
      nodeconf.vm.box = ubuntu_box
      nodeconf.vm.network :private_network, ip: "172.37.17.#{i+50}"

      # Disable default synced folder
      nodeconf.vm.synced_folder '.', '/vagrant', disabled: true

      # Run Ansible after all VMs are up
      if i == nodes
        nodeconf.vm.provision :ansible do |a|
          # Remove default inventory limit
          a.limit = 'all'
          # Specify master playbook
          a.playbook = "cluster.yml"
        end
      end

      nodeconf.vm.provider :virtualbox do |vbox|
        vbox.name = "ops#{i}.vm"
      end
    end
  end
end
