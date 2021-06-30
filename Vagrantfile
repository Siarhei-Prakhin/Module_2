Vagrant.configure("2") do |config|
  config.vm.box = "centos/8"
  config.vm.box_check_update = false

  config.vm.define "node1" do |node1|
    node1.vm.provider "virtualbox" do |vb|
      vb.name = "node1VBname"
    end
    node1.vm.hostname = "node1VMname"
    node1.vm.network "private_network", ip: "192.168.10.5"
    node1.vm.network "public_network"
    node1.vm.provision "shell", inline: "
        yum install git -y
        cd /home/vagrant
        git clone https://github.com/Siarhei-Prakhin/Module_2.git -b task2
        cat /home/vagrant/Module_2/test.txt
        echo 192.168.10.6 node2 >> /etc/hosts
    "
  end


  config.vm.define "node2" do |node2|
    node2.vm.provider "virtualbox" do |vb|
      vb.name = "node2VBname"
    end
    node2.vm.hostname = "node2VMname"
    node2.vm.network "private_network", ip: "192.168.10.6"
    node2.vm.network "public_network"
    node2.trigger.after :up do |trigger1|
      trigger1.run = {inline: "bash -c 'vagrant scp ./.vagrant/machines/node2/virtualbox/private_key node1:/home/vagrant/.ssh/id_rsa'"}
    end
    node2.trigger.after :up do |trigger2|
      trigger2.run = {inline: "bash -c 'vagrant scp ./.vagrant/machines/node1/virtualbox/private_key node2:/home/vagrant/.ssh/id_rsa'"}
    end

    node2.vm.provision "shell", inline: "
        echo 192.168.10.5 node1 >> /etc/hosts
    "
  end

end

