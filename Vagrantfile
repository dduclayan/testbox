Vagrant.configure("2") do |config|
  config.vm.define "testserver01" do |testserver01|
    testserver01.vm.box = "centos/8"
    testserver01.vm.network "private_network", ip: "192.168.10.10"
    testserver01.vm.hostname = "testserver01"
    testserver01.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "2048"]
      vb.customize ["modifyvm", :id, "--cpus", "2"]
      # set virtualbox gui name
      vb.name = "testserver01"
    end
    testserver01.vm.provision "shell", inline: <<-SHELL
      sudo timedatectl set-timezone America/Chicago
      sudo echo "192.168.1.11 testserver02" | sudo tee -a /etc/hosts
      sudo systemctl enable firewalld.service --now
      sudo yum install vim -y
      sudo useradd user
      sudo echo "password" | passwd --stdin user
    SHELL
  end
  config.vm.define "testserver02" do |testserver02|
    testserver02.vm.box = "centos/8"
    testserver02.vm.network "private_network", ip: "192.168.10.11"
    testserver02.vm.hostname = "testserver02"
    testserver02.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "2048"]
      vb.customize ["modifyvm", :id, "--cpus", "2"]
      # set virtualbox gui name
      vb.name = "testserver02"
    end
    testserver02.vm.provision "shell", inline: <<-SHELL
      sudo timedatectl set-timezone America/Chicago
      sudo echo "192.168.1.10 testserver01" | sudo tee -a /etc/hosts
      sudo systemctl enable firewalld.service --now
      sudo yum install vim -y
      sudo useradd user
      sudo echo "password" | passwd --stdin user
      sudo yum groups install "server with gui" -y
      sudo systemctl set-default graphical.target
      sudo systemctl start graphical.target
    SHELL
  end
end
