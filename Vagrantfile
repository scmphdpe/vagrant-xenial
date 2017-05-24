Vagrant.configure('2') do |config|
  config.ssh.insert_key = true
  config.ssh.forward_agent = true
  config.vm.box = 'bento/ubuntu-16.04'
  config.vm.box_check_update = true
  config.vm.network 'forwarded_port', guest: 80, host: 8080
  if Vagrant.has_plugin?('vagrant-proxyconf')
    config.proxy.http = ENV.fetch('HTTP_PROXY', '')
    config.proxy.https = ENV.fetch('HTTPS_PROXY', '')
    config.proxy.no_proxy = ENV.fetch('NO_PROXY', '')
  end
  config.vm.provider 'virtualbox' do |virtualbox|
    virtualbox.gui = true
    virtualbox.name = 'xenial'
    virtualbox.customize ['modifyvm', :id, '--memory', 8192]
    virtualbox.customize ['modifyvm', :id, '--vram', 64]
    virtualbox.customize ['modifyvm', :id, '--accelerate3d', 'on']
    virtualbox.customize ['modifyvm', :id, '--natdnshostresolver1', 'on']
    virtualbox.customize ['modifyvm', :id, '--natdnsproxy1', 'on']
  end
  config.vm.provision :shell, privileged: true, inline: <<-SHELL
    apt-get update
    apt-get install git unzip curl libxss1 ubuntu-desktop gnome-terminal indicator-session unity-lens-applications --yes --no-install-recommends
  SHELL
  config.vm.provision :shell, privileged: false, inline: <<-SHELL
    mkdir -p ~/.config/autostart
    mkdir -p ~/.ssh
    echo -e "Host *\n\tStrictHostKeyChecking no\n" >> ~/.ssh/config
    echo -e "[Desktop Entry]\nType=Application\nExec=gnome-terminal --working-directory="/home/vagrant"\nHidden=false\nNoDisplay=false\nX-GNOME-Autostart-enable=true\nName[en_US]=Terminal\nName=Terminal\nComment[en_US]=\nComment=\n" > ~/.config/autostart/gnome-terminal.desktop
    sudo sed -ie '/^XKBLAYOUT=/s/".*"/"it"/' /etc/default/keyboard && udevadm trigger --subsystem-match=input --action=change
  SHELL
end
