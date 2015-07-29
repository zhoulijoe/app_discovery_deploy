# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"


Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true

  config.vm.box = "ubuntu/trusty64"

  # Setup db server first, so API servlet can connect to DB and initialize correctly
  config.vm.define "app-discovery-db" do |db|
    db.vm.hostname = "app-discovery-db"
    db.vm.network "private_network", ip: "10.20.2.3"
  end

  config.vm.define "app-discovery-api", primary: true do |api|
    api.vm.hostname = "app-discovery-api"
    api.vm.network "private_network", ip: "10.20.2.2"
    api.vm.network "public_network", bridge: "en0: Wi-Fi (AirPort)"
    if FileTest::directory?("../app_discovery_server/build/libs")
      api.vm.synced_folder "../app_discovery_server/build/libs/", "/opt/app_discovery_server/"
    end
  end

  # Configure local DNS for vagrant.yml boxes
  config.vm.provision "hosts" do |provisioner|
    provisioner.add_host "10.20.2.2", ["app-discovery-api"]
    provisioner.add_host "10.20.2.3", ["app-discovery-db"]
  end

  config.vm.provision "ansible" do |ansible|
    ansible.sudo = true
    ansible.playbook = "api_server.yml"
    ansible.groups = {
        "api" => ["app-discovery-api"],
        "db" => ["app-discovery-db"],
        "vagrant.yml:children" => ["api", "db"]
    }
  end
end
