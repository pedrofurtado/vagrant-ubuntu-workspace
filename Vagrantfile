Vagrant.configure('2') do |config|
  config.vm.box = 'ubuntu/jammy64'
  config.vagrant.plugins = ['vagrant-vbguest']
  config.vm.provider 'virtualbox' do |v|
    v.memory    = '1024'
    v.cpus      = '1'
  end
  config.vm.provision 'shell', inline: <<-SHELL
    export DEBIAN_FRONTEND=noninteractive
    export DEBCONF_NONINTERACTIVE_SEEN=true

    sudo apt-get update -y

    sudo apt-get install -y dos2unix

    sudo apt-get install -y jq

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
    sudo pip3 install docker==6.1.3

    sudo docker volume create portainer_data
    sudo docker container run -d -p 9999:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce

    sudo apt-get install -y zip unzip wget
    wget https://bin.equinox.io/c/bNyj1mQVY4c/ngrok-v3-stable-linux-amd64.tgz
    sudo tar xvzf ngrok-v3-stable-linux-amd64.tgz -C /usr/bin
    sudo rm -f ngrok-v3-stable-linux-amd64.tgz

    curl -Lo ./kind https://kind.sigs.k8s.io/dl/latest/kind-linux-amd64
    sudo chmod +x ./kind
    sudo mv ./kind /usr/local/bin/kind

    curl -s https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | TAG=v5.6.3 bash

    sudo apt-get update -y
    sudo apt-get install -y ca-certificates curl apt-transport-https
    curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
    sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
    rm -f ./kubectl

    sudo apt-get install -y virtualbox-guest-additions-iso
    sudo apt-get install -y virtualbox-guest-dkms
    sudo apt-get install -y virtualbox-guest-utils
    sudo apt-get install -y virtualbox-guest-x11

    sudo wget https://github.com/aws/aws-sam-cli/releases/latest/download/aws-sam-cli-linux-x86_64.zip
    sudo unzip aws-sam-cli-linux-x86_64.zip -d sam-installation
    sudo ./sam-installation/install
    sudo rm -Rf ./sam-installation/ aws-sam-cli-linux-x86_64.zip

    sudo curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
    sudo unzip awscliv2.zip
    sudo ./aws/install
    sudo rm -Rf ./aws/ awscliv2.zip

    export EKSCTL_ARCH=amd64
    export EKSCTL_PLATFORM=$(uname -s)_$EKSCTL_ARCH
    sudo curl -sLO "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_$EKSCTL_PLATFORM.tar.gz"
    sudo tar -xzf eksctl_$EKSCTL_PLATFORM.tar.gz -C /tmp
    sudo rm -f eksctl_$EKSCTL_PLATFORM.tar.gz
    sudo mv /tmp/eksctl /usr/local/bin

    sudo apt-get install -y p7zip-full

    curl -sL https://deb.nodesource.com/setup_20.x | sudo -E bash -
    sudo apt-get install -y nodejs
    npm install -g nodemon

    curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
    sudo chmod 700 get_helm.sh
    sudo ./get_helm.sh
    rm -f ./get_helm.sh

    sudo apt-get install -y php
    sudo apt-get install -y php-curl
    sudo php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
    sudo php composer-setup.php
    sudo mv composer.phar /usr/local/bin/composer
    sudo php -r "unlink('composer-setup.php');"

    sudo apt-get install -y putty-tools

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

sudo cat >> /k8s_create_cluster.sh <<'EOL'
#!/bin/bash
kubectl delete all --all
kubectl delete "$(kubectl api-resources --namespaced=true --verbs=delete -o name | tr "\n" "," | sed -e 's/,$//')" --all
kubectl delete all --all --all-namespaces
kind delete cluster
kind create cluster
kubectl cluster-info --context kind-kind
EOL

    sudo chmod +x /k8s_create_cluster.sh
    sudo echo "alias k8s_create_cluster='/k8s_create_cluster.sh'" >> /root/.bashrc

    sudo cat >> /k8s_create_dashboard.sh <<'EOL'
#!/bin/bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml
sleep 10
kubectl create serviceaccount -n kubernetes-dashboard admin-user
kubectl create clusterrolebinding -n kubernetes-dashboard admin-user --clusterrole cluster-admin --serviceaccount=kubernetes-dashboard:admin-user
token=$(kubectl -n kubernetes-dashboard create token admin-user)
echo "Use this token inside K8s dashboard setup page: $token"
EOL

    sudo chmod +x /k8s_create_dashboard.sh
    sudo echo "alias k8s_create_dashboard='/k8s_create_dashboard.sh'" >> /root/.bashrc

    sudo cat >> /k8s_start_dashboard.sh <<'EOL'
#!/bin/bash
echo "Please, access the URL: http://localhost:9998/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login"
kubectl proxy --port=9998 --address=0.0.0.0
EOL

    sudo chmod +x /k8s_start_dashboard.sh
    sudo echo "alias k8s_start_dashboard='/k8s_start_dashboard.sh'" >> /root/.bashrc

    echo "Workspace created!"
  SHELL
end
