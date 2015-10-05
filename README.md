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

### Hyper-V
Instructions to follow!!

### Virtualbox
Instructions to follow!!

## Running the Docker Reveal Container & Presentation
Instructions to follow!!

## Updating the Presentation
__Work In Progress__

The current presentation is only using the index.html file in this directory.
Changes to this file will be reflected in the presentation upon refreshing the
browser it is running in. The watcher started by the ```grunt serve``` command 
is responsible for this.