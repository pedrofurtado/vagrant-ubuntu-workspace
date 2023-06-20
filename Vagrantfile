Vagrant.configure('2') do |config|
  config.vm.box = 'ubuntu/jammy64'
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

    sudo apt-get install -y unzip wget
    wget https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-linux-amd64.zip
    unzip ngrok-stable-linux-amd64.zip
    sudo mv ./ngrok /usr/bin/ngrok

    curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-linux-amd64
    sudo chmod +x ./kind
    sudo mv ./kind /usr/local/bin/kind

    sudo apt-get update -y
    sudo apt-get install -y ca-certificates curl apt-transport-https
    curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-archive-keyring.gpg
    echo "deb [signed-by=/etc/apt/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
    sudo apt-get update -y
    sudo apt-get install -y kubectl

    sudo apt-get install -y virtualbox-guest-additions-iso
    sudo apt-get install -y virtualbox-guest-dkms
    sudo apt-get install -y virtualbox-guest-utils
    sudo apt-get install -y virtualbox-guest-x11

    sudo echo "alias dc_down='docker-compose down --volumes --rmi local --remove-orphans'" >> /root/.bashrc
    sudo echo "alias dc_up='docker-compose up --build -d'" >> /root/.bashrc
    sudo echo "alias dc_logs='docker-compose logs -f --tail 100'" >> /root/.bashrc
    sudo echo "alias dc_exec='docker container exec -it'" >> /root/.bashrc
    sudo echo "alias dc_restart='docker-compose restart'" >> /root/.bashrc
    sudo echo "alias dc_stop='docker-compose stop'" >> /root/.bashrc
    sudo echo "alias dc_start='docker-compose start'" >> /root/.bashrc

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
