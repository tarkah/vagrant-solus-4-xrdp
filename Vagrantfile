# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrant file for setting up a build environment to test xrdp on Solus. 
# Copy xrdp & xorgxrdp eopkg files to ./share

Vagrant.configure(2) do |config|
  config.vm.box = "tarkah/solus-budgie-4"
  config.vm.box_version = "4.0.0"

  config.vm.provider "virtualbox" do |v|
    v.cpus = `nproc`.to_i
    v.memory = `grep 'MemTotal' /proc/meminfo | sed -e 's/MemTotal://' -e 's/ kB//'`.to_i / 1024 / 2
  end

  config.vm.network "forwarded_port", guest: 3389, host: 33389
  config.vm.network "forwarded_port", guest: 3389, host: 33389, protocol: "udp"


  config.vm.provision "file", source: "./share", destination: "/home/vagrant/share"

  config.vm.provision "shell", privileged: "true", inline: <<-SHELL
    eopkg it -y /home/vagrant/share/xrdp-*.eopkg /home/vagrant/share/xorgxrdp-*.eopkg
    systemctl enable xrdp
    systemctl start xrdp
    systemctl status xrdp    
  SHELL

end
