# practera_provider
practera in docker provider

Vagrantfile + docker-compose.yml are replaced with Vagrantfile + DockerHostVagrantfile.

Vagrantfile is about containers.

DockerHostVagrantfile is about Host VM that is running the docker containers. Host VM will be called by each container when container is being created.
