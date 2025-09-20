Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/jammy64"
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.network "private_network", type: "dhcp", auto_config: false

  config.vm.define "vam1" do |vm1|
    vm1.vm.hostname = "vm1"
    vm1.vm.network "private_network", ip: "192.168.10.10"
    vm1.vm.network "forwarded_port", guest: 3389, host: 3389

    vm1.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update -y
      sudo apt-get upgrade -y

      sudo apt-get install -y openssh-server

      echo "Protocol 2" | sudo tee -a /etc/ssh/sshd_config
      echo "MaxAuthTries 2" | sudo tee -a /etc/ssh/sshd_config
      echo "LoginGraceTime 10s" | sudo tee -a /etc/ssh/sshd_config
      echo "MaxSessions 2" | sudo tee -a /etc/ssh/sshd_config
      echo "PermitEmptyPasswords no" | sudo tee -a /etc/ssh/sshd_config
      echo "PasswordAuthentication no" | sudo tee -a /etc/ssh/sshd_config
      echo "HostBasedAuthentication no" | sudo tee -a /etc/ssh/sshd_config
      echo "PermitRootLogin no" | sudo tee -a /etc/ssh/sshd_config
      echo "PubkeyAuthentication yes" | sudo tee -a /etc/ssh/sshd_config

      echo "ubuntu:123456ubuntu" | sudo chpasswd
      
      sudo apt-get install -y ubuntu-desktop-minimal
      sudo apt-get install -y snap-store
      sudo apt-get install -y chromium
      sudo apt-get install -y xrdp
      sudo systemctl enable xrdp
      sudo systemctl start xrdp

      echo "192.168.10.20 vm2" | sudo tee -a /etc/hosts
      echo "192.168.10.30 vm3" | sudo tee -a /etc/hosts

      sudo systemctl restart ssh
    SHELL
  end

  config.vm.define "vam2" do |vm2|
    vm2.vm.hostname = "vm2"
    vm2.vm.network "private_network", ip: "192.168.10.20"
    vm2.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update -y
      sudo apt-get upgrade -y
      sudo apt-get install -y openssh-server

      echo "Protocol 2" | sudo tee -a /etc/ssh/sshd_config
      echo "MaxAuthTries 2" | sudo tee -a /etc/ssh/sshd_config
      echo "LoginGraceTime 10s" | sudo tee -a /etc/ssh/sshd_config
      echo "MaxSessions 2" | sudo tee -a /etc/ssh/sshd_config
      echo "PermitEmptyPasswords no" | sudo tee -a /etc/ssh/sshd_config
      echo "PasswordAuthentication no" | sudo tee -a /etc/ssh/sshd_config
      echo "HostBasedAuthentication no" | sudo tee -a /etc/ssh/sshd_config
      echo "PermitRootLogin no" | sudo tee -a /etc/ssh/sshd_config
      echo "PubkeyAuthentication yes" | sudo tee -a /etc/ssh/sshd_config

      echo "ubuntu:123456ubuntu" | sudo chpasswd

      sudo apt-get update
      sudo apt-get install ca-certificates curl
      sudo install -m 0755 -d /etc/apt/keyrings
      sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
      sudo chmod a+r /etc/apt/keyrings/docker.asc
      echo \
        "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
        $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
      sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
      sudo apt-get update
      sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
      sudo docker run hello-world

      sudo apt-get install -y wireguard-tools
      sudo mkdir -p /etc/wireguard
      cd /etc/wireguard
      sudo curl -L https://github.com/ngoduykhanh/wireguard-ui/releases/latest/download/wireguard-ui-linux-amd64 -o wireguard-ui
      sudo chmod +x wireguard-ui
      sudo ./wireguard-ui &

     sudo tee /etc/systemd/system/wireguard-ui.service <<EOF
    
     [Unit]
      Description=WireGuard UI
      After=network.target

    [Service]
      ExecStart=/etc/wireguard/wireguard-ui
      User=ubuntu
      Restart=always
      RestartSec=5

    [Install]
    WantedBy=multi-user.target
EOF

    sudo systemctl enable wireguard-ui
    sudo systemctl start wireguard-ui

      sudo systemctl restart ssh
   SHELL
  end

  config.vm.define "vam3" do |vm3|
    vm3.vm.hostname = "vm3"
    vm3.vm.network "private_network", ip: "192.168.10.30"
    vm3.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update -y
      sudo apt-get upgrade -y
      sudo apt-get install -y openssh-server

      echo "Protocol 2" | sudo tee -a /etc/ssh/sshd_config
      echo "MaxAuthTries 2" | sudo tee -a /etc/ssh/sshd_config
      echo "LoginGraceTime 10s" | sudo tee -a /etc/ssh/sshd_config
      echo "MaxSessions 2" | sudo tee -a /etc/ssh/sshd_config
      echo "PermitEmptyPasswords no" | sudo tee -a /etc/ssh/sshd_config
      echo "PasswordAuthentication no" | sudo tee -a /etc/ssh/sshd_config
      echo "HostBasedAuthentication no" | sudo tee -a /etc/ssh/sshd_config
      echo "PermitRootLogin no" | sudo tee -a /etc/ssh/sshd_config
      echo "PubkeyAuthentication yes" | sudo tee -a /etc/ssh/sshd_config

      echo "ubuntu:123456ubuntu" | sudo chpasswd

      sudo useradd -m -s /bin/bash -G sudo adam
      echo "adam ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/adam
      sudo systemctl restart ssh
    SHELL
  end

end
