# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

DOCKER_HOST_NAME = "Practera Docker Host" 
 
DOCKER_HOST_VAGRANTFILE = "./DockerHostVagrantfile" 



Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

######data######

   config.vm.define "data" do |v|             ### Name of VM
     v.vm.provider "docker" do |d|
       d.vagrant_machine = "#{DOCKER_HOST_NAME}"
       d.vagrant_vagrantfile = "#{DOCKER_HOST_VAGRANTFILE}"
       d.name = "practera-data"               ### Name of Container
       d.remains_running = true
       d.build_dir = "docker/develop/data"    ### The path of data
#       d.build_args = ["-t=data"]            ### Tag
     end
     v.vm.synced_folder ".", "/app"           ### sync folders from the host Vagrant is running
   end

######postgres######

   config.vm.define "postgres" do |v|
     v.vm.provider "docker" do |d|
       d.vagrant_machine = "#{DOCKER_HOST_NAME}"
       d.vagrant_vagrantfile = "#{DOCKER_HOST_VAGRANTFILE}"
       d.name = "practera-postgres"       
       d.remains_running = true
       d.image = "postgres:9.4"
#       d.build_args = ["-t=postgres"]
       d.ports = ["5432:5432"]
       d.create_args = ["--volumes-from=practera-data"]
       d.env = {
        LC_ALL: "C.UTF-8",
        PGDATA: "/pgdata",
       }
     end
     v.vm.synced_folder "./docker/develop/postgres/entrypoint", "/docker-entrypoint-initdb.d"
   end

######redis#####

   config.vm.define "redis" do |v|
     v.vm.provider "docker" do |d|
       d.vagrant_machine = "#{DOCKER_HOST_NAME}"
       d.vagrant_vagrantfile = "#{DOCKER_HOST_VAGRANTFILE}"
       d.name = "practera-redis"       
       d.remains_running = true
       d.image = "redis"
#       d.build_args = ["-t=redis"]
     end
   end

######web######

   config.vm.define "web" do |v|
     v.vm.provider "docker" do |d|
       d.vagrant_machine = "#{DOCKER_HOST_NAME}"
       d.vagrant_vagrantfile = "#{DOCKER_HOST_VAGRANTFILE}"
       d.name = "practera-web"       
       d.remains_running = true
       d.image = "350544449840.dkr.ecr.ap-southeast-2.amazonaws.com/practera/develop:latest"
#       d.build_args = ["-t=web"]
       d.ports = ["80:80"]
       d.link("practera-postgres:practera-postgres")
       d.link("practera-redis:practera-redis")

###Make sure sshd run in the container started, otherwise it will stick in booting state.
       d.has_ssh = true

       d.create_args = ["--dns=8.8.8.8", "--dns=8.8.4.4", "--volumes-from=practera-data"]
     end
   end





#####analytics#####

   config.vm.define "analytics" do |v|
     v.vm.provider "docker" do |d|
       d.vagrant_machine = "#{DOCKER_HOST_NAME}"
       d.vagrant_vagrantfile = "#{DOCKER_HOST_VAGRANTFILE}"
       d.name = "practera-analytics"       
       d.remains_running = true
       d.image = "350544449840.dkr.ecr.ap-southeast-2.amazonaws.com/practera/analytics:latest"
#       d.build_args = ["-t=analytics"]
       d.ports = ["8787:8787"]
       d.link("practera-postgres:practera-postgres")
#       d.has_ssh = true
       d.create_args = ["--volumes-from=practera-data"]
     end
   end
end