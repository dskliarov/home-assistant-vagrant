# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

  config.vm.box = "centos/7"

  config.vm.provider :virtualbox do |vb|
    vb.memory = 1024
    vb.cpus = 1
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
  end

  config.vm.define :hass1 do |host|
    host.vm.hostname = "hass1"
    # NOTE: You can override the boxtype here
    host.vm.network "forwarded_port", guest: 8123, host: 8123
    host.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--name", host.vm.hostname]
    end
  end

  config.vm.provision :ansible do |ansible|
    ansible.groups = {
        "app" => ["hass1"]
    }
    ansible.playbook = "playbook.yml"
    ansible.galaxy_role_file = "roles/requirements.yml"
    ansible.galaxy_command = "ansible-galaxy install --role-file=%{role_file} --roles-path=roles/ --ignore-errors"
    ansible.extra_vars = { ansible_winrm_scheme: "http" }
  end
end

