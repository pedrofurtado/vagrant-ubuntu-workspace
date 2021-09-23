Vagrant.configure('2') do |config|
  config.vm.box = 'ubuntu/focal64'
  config.vagrant.plugins = ['vagrant-vbguest']
  config.vm.provision 'shell', inline: <<-SHELL
    export DEBIAN_FRONTEND=noninteractive
    export DEBCONF_NONINTERACTIVE_SEEN=true

    sudo apt-get update -y

    sudo apt-get install -y dos2unix

    echo "sudo timedatectl set-timezone America/Sao_Paulo" >> /home/vagrant/.profile

    sudo apt-get install -y apt-transport-https ca-certificates curl gnupg lsb-release
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
    echo \
    "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    sudo apt-get update -y
    sudo apt-get install -y docker-ce docker-ce-cli containerd.io

    sudo apt-get install -y python3-pip libffi-dev
    sudo pip3 install docker-compose

    sudo docker volume create portainer_data
    sudo docker container run -d -p 9999:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce

    sudo apt-get install -y virtualbox-guest-additions-iso
    sudo apt-get install -y virtualbox-guest-dkms
    sudo apt-get install -y virtualbox-guest-utils
    sudo apt-get install -y virtualbox-guest-x11

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
cd /vagrant
sudo su
source /etc/profile.d/workspace.sh
EOL

    sudo cat >> /usr/bin/dos2unix_recursive <<'EOL'
#!/bin/bash
find $1 -type f -print0 | xargs -0 dos2unix
EOL
    sudo chmod +x /usr/bin/dos2unix_recursive

    echo "Workspace created!"
  SHELL
end
