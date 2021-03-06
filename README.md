# imx_install_poky

This container's single purpsoe is to pull down the Yocto SDK 'Poky' to the container or host mapped volume. 

It is flexible in that if you map a volume to /root/nxp on your host machine, the full Yocto Poky SDK
will be copied to that volume on the host.

There are two modes of operation:
1) interactive
2) non-interactive

# Interactive mode
This mode launches a simple terminal based script that presents you a menu of options.
Choosing option 1) Install Yocto Poky will guide you through a simple install process. 

it will give you the option of running oe-init-build-env with just defaults as well.

# How to run in interactive mode
In this example i have included a host volume in the command string.  You can omit the volume if you do not want it

    docker run -i -v /mypath/mydir:/root/poky <imx_install_poky> --interactive
note: the interactive command line argument can either be -i or --interactive

The continer will execute and the menu system will appear in your terminal.
select option 1, hit enter and the process will begin.

Once you exit the menu, the container stops.

If you mapped a host volume, then the full Yocto Poky SDK will reside in that volume for you to use after
rm'ing the container.

# Non-interactive mode
This mode will run straight through installing the Yocto Poky SDK. 

Non-interactive mode allows command line arguments to choose a specific Yocto Project SDK to download or you can accept the defaults within the container (which are at this time the sumo version of Poky)

# how to run in non-interactive mode
Non-interactive mode instructs the container to download the yocto Poky sdk to /root/nxp/poky inside the container.

the full command line arguments include the git source and the version you want.   both are optional.
if no command line arguments are included then the container defaults will be used.

arguments:
docker run -i <imx_install_poky> -v <version> -s <git source>
  option:  
  version can be written as -v or --version
  source can be written as -s our --source

# to run non-interactive mode with container defaults:

    docker run -i -v /mypath/mydir:/root/poky <imx_install_poky>

this will pull Poky from : git://git.yoctoproject.org/poky

# to run non-interactive mode with your command line argument choices:

specify the source where source = git://git.yoctoproject.org/poky

    docker run -i <imx_install_poky> --source git://gityoctoproject.org/poky

specify the version and source where version = sumo and source = git://gityoctoproject.org/poky

    docker run -i <imx_install_poky> --version sumo --source git://gityoctoproject.org/poky

You have the option to only specify source.  if version is left out, the container will ignore the field
for example

    docker run -i <imx_install_poky> --source git://gityoctoproject.org/poky
    
this will grab the latest version of Poky

if you only specify the version, the container will default to a source of git://gityoctorpoject.org/poky

for example:

    docker run -i <imx_install_poky> --version sumo
    
this will pull the sumo version from git://gityoctoproject.org/poky

This option is useful if you want to move back and forth between versions of poky.

# How to build
simple clone the project to your local host and run the following:

Makefile based.  Here are some simple build options you can use:

$ make build <- builds the container

$ make run <- runs a test container with default options

$ make build run <- builds the container and runs it in a test mode

$ make shell <-- after performing a make run, executing a make shell command will docker exec -it bash to the running container

"cleanrun" based
I created this script to do a complete build after docker rm of all containers on your host.
This made my life a bit easier with dependencies and cluttered up containers with various builds.
WARNING:  running ./cleanrun WILL automatically stop all containers, rm all containers and will not give you an option to stop!!!!  


