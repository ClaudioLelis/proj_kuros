Vagrant.configure("2") do |config|
    config.vm.box = "hashicorp/bionic64"
  
    config.vm.provider :virtualbox do |v|
      #v.memory = 2048
      v.memory = 32768
    end
  
    # synced folder 
    config.vm.synced_folder ".", "/vagrant"
    
    # Update repositories
    config.vm.provision 'shell', inline: <<-'SCRIPT'
      sudo apt-get update -y
      sudo apt-get install -y --no-install-recommends --no-install-suggests apt-transport-https ca-certificates curl gnupg lsb-release
      curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
      echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    SCRIPT
    
    
    config.vm.provision 'shell', inline: <<-'SCRIPT'
      sudo apt-get update -y
      sudo apt-get install -y --no-install-recommends --no-install-suggests docker-ce docker-ce-cli containerd.io
      dockerd &> dockerd-logfile.txt &
    SCRIPT

    # Upgrade installed packages
    #config.vm.provision :shell, inline: "sudo apt-get upgrade -y"
  
    # Add `vagrant` to Administrator
    #config.vm.provision :shell, inline: "sudo usermod -a -G sudo vagrant"
    
    config.vm.provision 'shell', inline: <<-'SCRIPT'
      sudo apt-get install -y --no-install-recommends --no-install-suggests ansible git aptitude
      git clone https://github.com/containernet/containernet.git
      cd containernet/ansible
      sudo ansible-playbook -i \"localhost,\" -c local install.yml
      cd ..
      sudo make develop
      docker pull andrekuros/px4-sitl-headless-flex
    SCRIPT
    # Restart
    #config.vm.provision :shell, inline: "sudo shutdown -r now"
end
