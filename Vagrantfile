# -*- mode: ruby -*-
# vi: set ft=ruby :

file_root = File.dirname(File.expand_path(__FILE__))

# u01_disk = File.join(file_root, 'disks/u01_disk.vdi')
# swap1_disk = File.join(file_root, 'disks/swap1_disk.vdi')

Vagrant.configure(2) do |config|

  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
  end

  config.vm.define "tomcat" do |tomcat|
    tomcat.vm.box = "ARTACK/debian-jessie"
    tomcat.vm.network "private_network", ip: "10.0.0.20"

    tomcat.vm.provider "virtualbox" do |vbox|
      vbox.cpus = 2
      vbox.memory = 4096
    end

    tomcat.vm.hostname = "tomcatserver"

    # Tomcat shares
    tomcat.vm.network "forwarded_port", guest: 8443, host: 8443
    tomcat.vm.synced_folder "shares/tomcat/webapps", "/var/tomcat/webapps", mount_options: ["uid=515,gid=515"]
    tomcat.vm.synced_folder "shares/tomcat/logs", "/var/tomcat/logs", mount_options: ["uid=515,gid=515"]

    # Mongodb shares
    tomcat.vm.network "forwarded_port", guest: 27017, host: 27017
    #tomcat.vm.synced_folder "shares/mongo/data", "/var/lib/mongodb", mount_options: ["uid=512,gid=512"]
    #tomcat.vm.synced_folder "shares/mongo/logs", "/var/log/mongodb", mount_options: ["uid=512,gid=512"]

    # Mysql shares
    tomcat.vm.network "forwarded_port", guest: 3306, host: 3306
    tomcat.vm.synced_folder "shares/mysql/data", "/var/lib/mysql", mount_options: ["uid=513,gid=513"]
    tomcat.vm.synced_folder "shares/mysql/logs", "/var/log/mysql", mount_options: ["uid=513,gid=513"]

    # Postgres shares
    tomcat.vm.network "forwarded_port", guest: 5432, host: 5432
    #tomcat.vm.synced_folder "shares/postgres/data", "/var/lib/postgresql", mount_options: ["uid=514,gid=514"]
    #tomcat.vm.synced_folder "shares/postgres/logs", "/var/log/postgresql", mount_options: ["uid=root,gid=514"]


    tomcat.vm.provision "ansible" do |ansible|
      ansible.playbook = "tomcatserver.yml"
    end
  end


  config.vm.define "build" do |build|
    build.vm.box = "ARTACK/debian-jessie"
    build.vm.network "private_network", ip: "10.0.0.21"

    build.vm.provider "virtualbox" do |vbox|
      vbox.cpus = 2
      vbox.memory = 4096
    end

    build.vm.hostname = "buildserver"



    # Nexus shares
    build.vm.network "forwarded_port", guest: 8081, host: 8081
    build.vm.synced_folder "shares/nexus/conf", "/var/nexus/conf", mount_options: ["uid=510,gid=510"]
    build.vm.synced_folder "shares/nexus/logs", "/var/nexus/logs", mount_options: ["uid=510,gid=510"]
    build.vm.synced_folder "shares/nexus/storage/releases", "/var/nexus/storage/releases",
                            mount_options: ["uid=510,gid=510"]
    build.vm.synced_folder "shares/nexus/storage/snapshots", "/var/nexus/storage/snapshots",
                            mount_options: ["uid=510,gid=510"]
    build.vm.synced_folder "shares/nexus/storage/thirdparty", "/var/nexus/storage/thirdparty",
                            mount_options: ["uid=510,gid=510"]

    # Gitlab shares
    build.vm.network "forwarded_port", guest: 8082, host: 8082
    build.vm.synced_folder "shares/gitlab/git-data", "/var/opt/gitlab/git-data",
                            mount_options: ["uid=511,gid=511"]

    # GOCD shares
    build.vm.network "forwarded_port", guest: 8153, host: 8153

    # Sonarqube shares
    build.vm.network "forwarded_port", guest: 9000, host: 9000

    # Jenkins shares
    build.vm.network "forwarded_port", guest: 8083, host: 8083
    build.vm.synced_folder "shares/jenkins/logs", "/var/log/jenkins",
                           mount_options: ["uid=516,gid=516"]

    # Teamcity shares
    build.vm.network "forwarded_port", guest: 8111, host: 8111
    build.vm.synced_folder "shares/teamcity/logs", "/opt/teamcity/TeamCity/logs",
                           mount_options: ["uid=517,gid=517"]


    build.vm.provision "ansible" do |ansible|
      ansible.playbook = "buildserver.yml"
    end

  end



end
