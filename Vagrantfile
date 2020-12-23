require 'yaml'

config = YAML.load_file(File.join(File.dirname(__FILE__), 'config.yml'))

base_box=config['environment']['base_box']
boxes = config['boxes']

Vagrant.configure("2") do |config|
  config.vm.box = base_box

  # Disable or Enable SMB Share
  config.vm.synced_folder ".", "/vagrant", disabled: false

  boxes.each do |node|
    config.vm.define node['name'] do |config|
      config.vm.hostname = node['name']

      update_ansible_hosts = <<SCRIPT
      mkdir -p /etc/ansible
      echo "#{node['name']}" >/etc/ansible/hosts
SCRIPT

	    config.vm.provider "hyperv" do |hv|
        hv.vmname = node['name']
        # With nested virtualization, at least 2 CPUs are needed.
        hv.cpus = node['cpu']
		    # With nested virtualization, at least 4GB of memory is needed.
		    hv.memory = node['mem']
        # Faster cloning and uses less disk space
        hv.linked_clone = true
        # Enable nested virtualization
        # h.enable_virtualization_extensions = true
	    end
      
      config.vm.provider "virtualbox" do |v|
        v.name = node['name']
	      v.linked_clone = true
        config.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'"

        v.customize ["modifyvm", :id, "--memory", node['mem']]
        v.customize ["modifyvm", :id, "--cpus", node['cpu']]
        v.customize ["modifyvm", :id, "--uartmode1", "disconnected" ]
        v.customize ["modifyvm", :id, "--nictype1", "Am79C973"]
        v.customize ["modifyvm", :id, "--nicpromisc1", "allow-all"]
        v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      end

      #config.vm.provision :shell, :inline => update_ansible_hosts

      config.vm.provision :ansible_local do |ansible|
        ansible.playbook = "ansible/playbook.yml"
      end

    end
  end
end
