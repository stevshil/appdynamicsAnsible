Vagrant.configure("2") do |config|
  HOME_DISK="/home/VMs/AppDynamics/appdserver.vdi"

  config.vm.define :server do | server |
    config.vm.provider "virtualbox" do |vb|
       vb.memory = "6144"
       vb.cpus = 2
       vb.name = "AppDynamics"
       unless File.exist?(HOME_DISK)
         vb.customize ['createhd', '--filename', HOME_DISK, '--size', 100 * 1024]
       end
       vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', HOME_DISK]
    end

    if ENV['HOSTNAME'] == 'tpsserv.localdomain'
      server.vm.network "public_network", ip: "192.168.10.123", bridge: "enp3s0"
    elsif ENV['HOSTNAME'] == 'tpslaptop.localdomain'
      server.vm.network "public_network", ip: "192.168.10.123", bridge: "wlp3s0"
    else
      server.vm.network "public_network", ip: "192.168.10.123"
    end

    server.vm.provision "shell", inline: "apt-get -y update; apt-get -y install python3 python3-pip python3-venv parted && python3 -m venv /root/ansible && source /root/ansible/bin/activate && pip install --upgrade pip && pip install ansible"
    server.vm.provision "shell", inline: "source /root/ansible/bin/activate && ansible-playbook -i /vagrant/environments/vagrant /vagrant/vagrant.yml"
  end

  config.vm.box = 'envimation/ubuntu-xenial-docker'

end
