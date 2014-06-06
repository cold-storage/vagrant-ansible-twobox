VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # This is a minimal example of Vagrant provisioning two boxes using Ansible.

  config.vm.box = "precise64"
  config.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/precise/current/precise-server-cloudimg-amd64-vagrant-disk1.box"

  # Looks like as of 1.5.1 you MUST disable the default forwarded_port
  # in order to specify your own.
  # https://github.com/mitchellh/vagrant/issues/3232
  config.vm.network :forwarded_port, guest: 22, host: 2222, id: "ssh", disabled: true

  config.vm.define :box107 do |cfg|
    cfg.vm.network :private_network, ip: "192.168.77.107"

    # Must disable default forwarded_port as per above or this won't work
    cfg.vm.network :forwarded_port, guest: 22, host: 2207, auto_correct: true

    # ansible_ssh_port must match this port
    cfg.ssh.port = "2207"
    cfg.vm.provider :virtualbox do |v|

      # the name in your ansible.groups or ansible.inventory_path must
      # match this name.
      v.name = "box107"
    end
  end

  config.vm.define :box105 do |cfg|
    cfg.vm.network :private_network, ip: "192.168.77.105"
    cfg.vm.network :forwarded_port, guest: 22, host: 2205, auto_correct: true
    cfg.ssh.port = "2205"
    cfg.vm.provider :virtualbox do |v|
      v.name = "box105"
    end
  end

  config.vm.provision "ansible" do |ansible|
    ansible.extra_vars = { ansible_ssh_user: 'vagrant' }
    ansible.verbose = 'vvvv'

    # Do EITHER ansible.groups OR ansible.inventory_path
    # the following ansible.groups will auto-generate the exact same config as
    # what is in the 'hosts' file.
    # The generated file will be in
    # .vagrant/provisioners/ansible/inventory/vagrant_ansible_inventory
    ansible.groups = {
      "all" => ["box107", "box105"]
    }

    #ansible.inventory_path = "hosts"

    #ansible.limit = 'all'
    ansible.playbook = "main.yml"
  end

end
