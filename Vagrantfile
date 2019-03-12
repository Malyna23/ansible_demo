servers=[
  {
    :hostname => "db",
    :ip => "192.168.56.5",
    :box => "centos/7",
    :ram => 1024,
    :cpu => 1
  },  
{
    :hostname => "web",
    :ip => "192.168.56.2",
    :box => "centos/7",
    :ram => 1024,
    :cpu => 1
  },
  {
    :hostname => "web1",
    :ip => "192.168.56.3",
    :box => "centos/7",
    :ram => 1024,
    :cpu => 1
  },
  {
    :hostname => "web2",
    :ip => "192.168.56.4",
    :box => "centos/7",
    :ram => 1024,
    :cpu => 1
  }
]

Vagrant.configure(2) do |config|
  servers.each do |machine|
      config.vm.define machine[:hostname] do |node|
          node.vm.box = machine[:box]
          node.vm.hostname = machine[:hostname]
          node.vm.network "private_network", ip: machine[:ip]
          node.vm.network "public_network", machine
          node.vm.provider "virtualbox" do |vb|
              vb.customize ["modifyvm", :id, "--memory", machine[:ram]]
          end
          if machine == servers[3]
            node.vm.provision "file", source: ".vagrant/machines", destination: "~/"
            node.vm.provision "shell", inline: <<-SHELL
            sudo chmod 600 /home/vagrant/machines/db/virtualbox/private_key
            sudo chmod 600 /home/vagrant/machines/web/virtualbox/private_key
            sudo chmod 600 /home/vagrant/machines/web1/virtualbox/private_key
            sudo chmod 600 /home/vagrant/machines/web2/virtualbox/private_key
           SHELL
            #node.vm.provision "file", source: ".vagrant/machines", destination: "~/"
            #node.vm.provision "shell", path: "ansible.sh"
          
          node.vm.provision :ansible_local do |ansible|
            ansible.playbook       = "playbook.yml"
            ansible.verbose        = true
            ansible.install        = true
            ansible.limit          = "all"
            ansible.inventory_path = "hosts.txt"
          
          end
        end
      end
  end
end
