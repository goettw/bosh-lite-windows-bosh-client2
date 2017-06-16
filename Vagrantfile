Vagrant.configure('2') do |config|
  config.vm.define "cf" do |cf|
    cf.vm.box = 'cloudfoundry/bosh-lite'
    cf.vm.provider :virtualbox do |v, override|
      override.vm.box_version = '9000.137.0' # ci:replace
      # To use a different IP address for the bosh-lite director, uncomment this line:
      # override.vm.network :private_network, ip: '192.168.59.4', id: :local
      cf.vm.synced_folder ".", "/vagrant", mount_options: ["dmode=777"] # ensure any VM user can create files in subfolders - eg, /vagrant/tmp
    end

  end
  
  

  config.vm.define "bosh-cli2" do |boshcli2|
    boshcli2.vm.provision "shell", inline: "apt-get update", privileged: true
    boshcli2.vm.provision "shell", inline: "apt-get -y install build-essential linux-headers-`uname -r`", privileged: true
    boshcli2.vm.provision "shell", inline: "sudo -E apt-get -y install git zip", privileged: true
    boshcli2.vm.provision "shell", inline: "wget https://s3.amazonaws.com/bosh-cli-artifacts/bosh-cli-2.0.23-linux-amd64 -O bosh;mv bosh /usr/local/bin;chmod +x /usr/local/bin/bosh", privileged: true
    boshcli2.vm.provision "shell", inline: "git clone https://github.com/cloudfoundry/bosh-lite.git", privileged: false
    boshcli2.vm.provision "shell", inline: "echo export BOSH_CLIENT=admin >> .bashrc", privileged: false
    boshcli2.vm.provision "shell", inline: "echo export BOSH_CLIENT_SECRET=admin >> .bashrc", privileged: false
    boshcli2.vm.provision "shell", inline: "bosh -e 192.168.50.4 --ca-cert <(cat bosh-lite/ca/certs/ca.crt) alias-env vbox", privileged: false
    boshcli2.vm.provision "shell", inline: "echo export BOSH_ENVIRONMENT=vbox >> .bashrc", privileged: false
    boshcli2.vm.provision "shell", inline: "git clone https://github.com/cloudfoundry/cf-deployment ~/workspace/cf-deployment", privileged: false
    
    boshcli2.vm.provider :virtualbox do |v, override|
      override.vm.box = 'ubuntu/trusty64'

      # To use a different IP address for the bosh-lite director, uncomment this line:
      override.vm.network :private_network, ip: '192.168.50.14', id: :local
      override.vm.network :public_network
      v.memory = 6144
      v.cpus = 2
    end
  end
end

