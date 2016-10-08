# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.define "Plex" do |plex|
    plex.vm.hostname = "plex"
    plex.vm.box = "ubuntu/xenial64"
    plex.vm.box_version = "20160930.0.0"

    plex.vm.synced_folder "/data/plex-home", "/var/lib/plexmediaserver/", type: "nfs", create: true
    plex.vm.synced_folder "/data/library",   "/var/lib/library"         , type: "nfs", create: true
    
    config.vm.network "private_network", type: "dhcp" # (For nfs)

    config.vm.network "forwarded_port", guest: 32400, host: 32400, protocol: "tcp" # (Plex main web/api)
    config.vm.network "forwarded_port", guest: 1900,  host: 1900,  protocol: "udp" # (Plex DLNA)
    config.vm.network "forwarded_port", guest: 32469, host: 32469, protocol: "udp" # (Plex DLNA)
    config.vm.network "forwarded_port", guest: 3005,  host: 3005,  protocol: "tcp" # (Plex Home Theater controls)
    config.vm.network "forwarded_port", guest: 5353,  host: 5353,  protocol: "udp" # (Plex local discovery)
    config.vm.network "forwarded_port", guest: 8324,  host: 8324,  protocol: "tcp" # (Roku via Plex Companion)
    config.vm.network "forwarded_port", guest: 32410, host: 32410, protocol: "udp" # (GDM network discovery)
    config.vm.network "forwarded_port", guest: 32412, host: 32412, protocol: "udp" # (GDM network discovery)
    config.vm.network "forwarded_port", guest: 32413, host: 32413, protocol: "udp" # (GDM network discovery)
    config.vm.network "forwarded_port", guest: 32414, host: 32414, protocol: "udp" # (GDM network discovery)

    plex.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
      vb.cpus = "4"
    end

    plex.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'"
    plex.vm.provision "shell", inline: <<-SHELL
      sudo echo "deb http://ubuntu.azuras.net/ trusty main" >> /etc/apt/sources.list.d/azuras-plexmediaserver.list
      sudo curl -qq http://shell.ninthgate.se/packages/shell.ninthgate.se.gpg.key | sudo apt-key add -
      sudo apt-get -qq update
      sudo apt-get -y --allow-unauthenticated install avahi-daemon plexmediaserver
    SHELL
  end
end
