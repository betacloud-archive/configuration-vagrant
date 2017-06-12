VERBOSE = false
PLAYBOOKS = [
  "001-pre-bootstrap.yml",
  "001-pre-bootstrap-custom.yml",
  "002-bootstrap-seed.yml",
  "003-bootstrap.yml",
  "004-bootstrap-kolla.yml",
  "005-prepare-post-bootstrap-custom.yml",
  "005-post-bootstrap-custom.yml",
  "006-pre-deployment-custom.yml"
]

Vagrant.configure("2") do |config|

  config.vm.define "ubuntu" do |ubuntu|
    ubuntu.vm.provider "virtualbox" do |v|
      v.memory = 24576
      v.cpus = 8
      v.customize ['modifyvm', :id, '--nictype1', 'virtio']
    end
    ubuntu.vm.network "private_network", ip: "192.168.50.10", nic_type: "virtio"
    ubuntu.vm.network "private_network", ip: "192.168.60.10", nic_type: "virtio"
    ubuntu.vm.network "private_network", ip: "192.168.70.10", nic_type: "virtio"
    ubuntu.vm.box = "cbednarski/ubuntu-1604-large"
    ubuntu.vm.hostname = "vagrant-betacloud"
    ubuntu.vm.synced_folder ".", "/opt/configuration"
    PLAYBOOKS.each do |playbook|
      ubuntu.vm.provision "ansible" do |ansible|
        ansible.verbose = VERBOSE
        ansible.playbook = playbook
        ansible.limit = "all"
        ansible.extra_vars = {
          ansible_os_family: "Debian",
          operator_user: "vagrant"
        }
        ansible.raw_arguments = ["--connection=paramiko"]
        ansible.groups = {
          "vagrant" => ["ubuntu"],
          "seed" => ["ubuntu"],
          "control" => ["ubuntu"],
          "compute" => ["ubuntu"],
          "storage" => ["ubuntu"],
          "network" => ["ubuntu"],
          "cobbler:children" => ["seed"],
          "elasticsearch:children" => ["seed"],
          "rabbitmq:children" => ["seed"],
          "iscsdi:children" => ["compute", "storage"],
        }
      end
    end
  end
end
