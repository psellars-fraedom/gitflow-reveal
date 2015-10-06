# GitFlow Reveal
GitFlow presentation using the [reveal.js](https://github.com/hakimel/reveal.js)
presentation framework. Information about creating and editing reveal.js 
presentations can be found in the [reveal.js](https://github.com/hakimel/reveal.js)
repository README.md file and via the example slides [here](http://lab.hakim.se/reveal-js/#/)

## Running the Presentation (In a Windows Environment)
This presentation has been developed using a [Docker](http://www.docker.com)
container running in a [Docker Machine](https://docs.docker.com/machine/) instance
in a Windows development environment.

## Installing and Starting a Docker Machine Instance

The easiest way to install Docker Machine and the Docker client for Windows is
to use [Docker Toolbox](https://docs.docker.com/installation/windows/). Once
you have installed the Docker Toolbox you will have access to the Windows Docker
client, Docker-Machine and Virtualbox.

If you would prefer not to install Virtualbox, as you prefer to use Hyper-V
then install the Docker Client and Docker Machine binaries from these
locations

* [Docker Client Binaries](https://docs.docker.com/installation/binaries/#get-the-windows-binary)
* [Docker Machine Binaries](https://github.com/docker/machine/releases/)

After installing the binaries check that they are working as expected - you should see output
similar to that displayed below for each of the binaries.

```bash
$ docker -v
Docker version 1.8.1, build d12ea79
```

```bash
$ docker-machine
Usage: C:\ProgramData\chocolatey\lib\docker-machine\bin\docker-machine.exe [OPTIONS] COMMAND [arg...]                               
                                                                                                                                    
Create and manage machines running Docker.                                                                                          
                                                                                                                                    
Version: 0.4.1 (e2c88d6)                                                                                                            
                                                                                                                                    
Author:                                                                                                                             
  Docker Machine Contributors - <https://github.com/docker/machine>                                                                 
                                                                                                                                    
Options:                                                                                                                            
  --debug, -D                                                   Enable debug mode                                                   
  -s, --storage-path "C:\Users\psellars\.docker\machine"        Configures storage path [$MACHINE_STORAGE_PATH]                     
  --tls-ca-cert                                                 CA to verify remotes against [$MACHINE_TLS_CA_CERT]                 
  --tls-ca-key                                                  Private key to generate certificates [$MACHINE_TLS_CA_KEY]          
  --tls-client-cert                                             Client cert to use for TLS [$MACHINE_TLS_CLIENT_CERT]               
  --tls-client-key                                              Private key used in client TLS auth [$MACHINE_TLS_CLIENT_KEY]       
  --native-ssh                                                  Use the native (Go-based) SSH implementation. [$MACHINE_NATIVE_SSH] 
  --help, -h                                                    show help                                                           
  --version, -v                                                 print the version                                                   
                                                                                                                                    
Commands:                                                                                                                           
  active                Print which machine is active                                                                               
  config                Print the connection config for machine                                                                     
  create                Create a machine                                                                                            
  env                   Display the commands to set up the environment for the Docker client                                        
  inspect               Inspect information about a machine                                                                         
  ip                    Get the IP address of a machine                                                                             
  kill                  Kill a machine                                                                                              
  ls                    List machines                                                                                               
  regenerate-certs      Regenerate TLS Certificates for a machine                                                                   
  restart               Restart a machine                                                                                           
  rm                    Remove a machine                                                                                            
  ssh                   Log into or run a command on a machine with SSH.                                                            
  scp                   Copy files between machines                                                                                 
  start                 Start a machine                                                                                             
  status                Get the status of a machine                                                                                 
  stop                  Stop a machine                                                                                              
  upgrade               Upgrade a machine to the latest version of Docker                                                           
  url                   Get the URL of a machine                                                                                    
  help, h               Shows a list of commands or help for one command                                                            
                                                                                                                                    
Run 'C:\ProgramData\chocolatey\lib\docker-machine\bin\docker-machine.exe COMMAND --help' for more information on a command.         
```

### Hyper-V
How to create a Docker Machine instance using the hyper-v driver

Please refer to some specific requirments of Hyper-V for use with
Docker Machine outlined [here](https://docs.docker.com/machine/drivers/hyper-v/)
before trying to create a Docker Machine instance.

```bash
$ docker-machine create --driver hyper-v presentation-vm
```

Check the instance was created

```bash
$ docker-machine ls --filter driver=hyper-v
NAME               ACTIVE   DRIVER    STATE     URL   SWARM
presentation-vm             hyper-v   Timeout
```

Start the instance

```bash
$ docker-machine start presentation-vm
Starting VM...
Started machines may have new IP addresses. You may need to re-run the `docker-machine env` command.
```

Check that it is running

```bash
$ docker-machine ls --filter driver=hyper-v
NAME                  ACTIVE   DRIVER    STATE     URL                         SWARM
presentation-vm                hyper-v   Running   tcp://192.168.99.100:2376
```

### Virtualbox
How to create a Docker Machine instance using the Virtualbox driver

```bash
$ docker-machine create --driver virtualbox presentation-vm
```

Check the instance was created
```bash
$ docker-machine ls --filter driver=virtualbox
NAME               ACTIVE   DRIVER       STATE     URL   SWARM
presentation-vm             virtualbox   Timeout
```

Start the instance

```bash
$ docker-machine start presentation-vm
Starting VM...
Started machines may have new IP addresses. You may need to re-run the `docker-machine env` command.
```

Check that it is running

```bash
$ docker-machine ls --filter driver=virtualbox
NAME                  ACTIVE   DRIVER       STATE     URL                         SWARM
presentation-vm                virtualbox   Running   tcp://192.168.99.100:2376
```

In order for the presentation to be available in a local web browser you will
need to enable port forwarding on port 9000. The simplest way to do this is to
set it via the network settings in the Virtualbox GUI.

TODO: Can this port be set via Docker Machine?

## Connecting to the Docker Machine Instance using the Docker Client
In order to pull and run the Docker Container used for the presentation framework
you need to connect to the Docker Machine instance with the Docker client. When
starting the Docker Machine instance it prompted you to check this, so lets do
that now.

Here we are using the ```--shell=cmd```, you may prefer to use the ```--shell=powershell```

```bash
$ docker-machine env presentation-vm --shell=cmd
set DOCKER_TLS_VERIFY=1
set DOCKER_HOST=tcp://192.168.99.100:2376
set DOCKER_CERT_PATH=C:\Users\psellars\.docker\machine\machines\presentation-vm
set DOCKER_MACHINE_NAME=presentation-vm
    # Run this command to configure your shell:
    # copy and paste the above values into your command prompt
```

As the output instructs copy and paste the values from the output into your
command prompt/powershell terminal. Your Docker Client should now be connected
to your Docker Machine instance.

```bash
$ docker -v
Docker version 1.8.1, build d12ea79

$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
```

## Pulling the Presentation Runner Container
This presentation is currently using the [dordoka/reveal](https://hub.docker.com/r/dordoka/reveal/)
container to run inside. Pull the latest image from the Docker Hub

```bash
$ docker pull dordoka/reveal

```

## Running the Docker Reveal Container & Presentation
Instructions to follow!!

## Updating the Presentation
__Work In Progress__

The current presentation is only using the index.html file in this directory.
Changes to this file will be reflected in the presentation upon refreshing the
browser it is running in. The watcher started by the ```grunt serve``` command 
is responsible for this.