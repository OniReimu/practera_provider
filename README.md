# practera_provider
practera in docker provider

Vagrantfile + docker-compose.yml are replaced with Vagrantfile + DockerHostVagrantfile.

Vagrantfile is about containers.

DockerHostVagrantfile is about Host VM that is running the docker containers. Host VM will be called by each container when container is being created.

Make sure you have your aws permission files.

Make sure you have your develop files in place.

Make sure you have enough permission accessing with your directory
  
    $ chmod 755 -R (Directory's path)
    
Make sure you have your 'config' and 'credentials' files in place that will be mounted inside the containers for aws authenication, that is docker/develop/vagrant/

RUN 
    $ vagrant up --provision --no-parallel
