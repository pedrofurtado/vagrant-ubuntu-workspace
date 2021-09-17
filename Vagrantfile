Vagrant.configure('2') do |config|
  config.vagrant.plugins = ['vagrant-vbguest', 'vagrant-disksize']
  config.vm.box          = 'ubuntu/focal64'
  config.disksize.size   = '60GB'

  config.vm.provider 'virtualbox' do |v|
    v.memory = '8192'
    v.cpus   = '2'
    v.customize ['setextradata', :id, 'VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root', '1']
  end

  config.vm.provision 'shell', inline: <<-SHELL
    export DEBIAN_FRONTEND=noninteractive
    export DEBCONF_NONINTERACTIVE_SEEN=true

    sudo apt-get update -y

    sudo apt-get install -y dos2unix

    echo "sudo timedatectl set-timezone America/Sao_Paulo" >> /home/vagrant/.profile

    echo "echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p" >> /home/vagrant/.profile

    sudo apt-get install -y apt-transport-https ca-certificates curl gnupg lsb-release
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
    echo \
    "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    sudo apt-get update -y
    sudo apt-get install -y docker-ce docker-ce-cli containerd.io

    sudo apt-get install -y jq
    export DOCKER_COMPOSE_VERSION=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | jq -r ".tag_name")
    sudo curl -L "https://github.com/docker/compose/releases/download/$DOCKER_COMPOSE_VERSION/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    sudo chmod +x /usr/local/bin/docker-compose
    sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose

    sudo touch /etc/profile.d/workspace.sh
    sudo chmod 0777 /etc/profile.d/workspace.sh
    cat >> /etc/profile.d/workspace.sh <<EOL
#!/bin/bash
export LANG=C.UTF-8
export TZ=America/Sao_Paulo
EOL
    sudo chmod +x /etc/profile.d/workspace.sh
    source /etc/profile.d/workspace.sh

    sudo cat >> /home/vagrant/.bashrc <<EOL
dos2unix /vagrant/**/*
cd /vagrant
sudo su
source /etc/profile.d/workspace.sh
EOL

    echo "Workspace created!"
  SHELL
end
