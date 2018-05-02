# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "ubuntu/xenial64"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  config.vm.synced_folder "/home/phelps/projects/", "/home/vagrant/projects/"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
   config.vm.provision "shell", inline: <<-SHELL
     apt-get update
     apt-get upgrade -y

     # General file editing
     apt-get install -y vim vim-doc meld exuberant-ctags python-pip virtualenv \
        screen tmux ipython cmake build-essential python-dev python3-dev golang \
        flake8 pylint pylint3

     # zsh
     apt-get install -y zsh zsh-common
     chsh -s $(which zsh) vagrant

     apt-get install -y libnss3-tools libbz2-dev libreadline-dev libssl-dev \
        zlib1g-dev
   SHELL

   $user_script = <<-USERSCRIPT
     pip install --upgrade pip
     pip install awscli --upgrade --user
     pip install setuptools wheel

     mkdir -p ~/.vim/bundle/
     git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim

     sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"

     curl -s https://raw.githubusercontent.com/thestinger/termite/master/termite.terminfo > /tmp/termite.terminfo
     tic /tmp/termite.terminfo

     GITHUB_DOTFILES=https://raw.githubusercontent.com/phelpsw/dotfiles/master
     curl -s $GITHUB_DOTFILES/gitconfig > ~/.gitconfig
     curl -s $GITHUB_DOTFILES/gitignore > ~/.gitignore
     curl -s $GITHUB_DOTFILES/screenrc > ~/.screenrc
     #curl -s $GITHUB_DOTFILES/vimrc > ~/.vimrc
     curl -s $GITHUB_DOTFILES/tmux.conf > ~/.tmux.conf
     curl -s $GITHUB_DOTFILES/zshrc > ~/.zshrc
     curl -s $GITHUB_DOTFILES/aliases > ~/.aliases

     vim +PluginInstall +qall
     pushd ~/.vim/bundle/YouCompleteMe
     #python ./install.py
     popd

     # Netflix tools
     curl -q -sL 'https://go.netflix.com/newt-install' | bash
     echo 'eval "$(newt --completion-script-zsh)"' >> .zshrc
     curl -sL https://go.netflix.com/metatron-install | bash -s -- --no-refresh
     echo "METATRON_USER=phelps metatron refresh"
     echo "Also, perform ssh-keygen and upload pubkey to stash!"
   USERSCRIPT
   config.vm.provision "shell", inline: $user_script, privileged: false

end
