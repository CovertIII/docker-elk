# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/trusty64"
  config.vm.network "private_network", ip: "192.168.59.105"

  if Vagrant::Util::Platform.windows?
    ENV["VAGRANT_DETECTED_OS"] = ENV["VAGRANT_DETECTED_OS"].to_s + " cygwin"
  end

  config.vm.synced_folder ".", 
    "/src", 
    type: "rsync", 
    rsync__exclude: [
      ".git/"
    ]

  config.vm.provider "virtualbox" do |v|
    v.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]
    v.memory = 2096
    v.cpus = 2
  end

  # Enable ssh forward agent
  config.ssh.forward_agent = true

  # need to run apt-get update
  config.vm.provision :shell, inline: "apt-get update"
  config.vm.provision :docker
  config.vm.provision :docker_compose, yml: "/src/docker-compose.yml", rebuild: true, project_name: "ra", run: "always"
end
