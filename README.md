# container-wrappers

Prototype for automated deployment of applications as bash wrappers around containers.

Assumes [Singularity](http://sylabs.io/singularity) is available in the system.

Find us on [GitHub](https://github.com/marcodelapierre/container-wrappers)


## Quick start

Edit the first few lines of the `setup.sh` script to provide values for the following variables:
* `imagedir`: location for downloaded container images
* `wrapdir`: location for application wrapper scripts
* `workdir`: work directory for production, where data are stored (can be comma separated list)

For Nimbus, additional steps are required:
* install modules
    ```
    wget https://sourceforge.net/projects/modules/files/Modules/modules-4.5.2/modules-4.5.2.tar.gz
    tar -xzvf modules-4.5.2.tar.gz
    cd modules-4.5.2
    ./configure
    make
    sudo make install
    ```
    You may need to install dependecies with  `sudo apt-get install tcl-dev tk-dev mesa-common-dev libjpeg-dev libtogl-dev dejagnu` if you run          into issues during installation.
    
* edit the `SINGULARITY_CACHEDIR` and `SINGULARITY_CACHEDIR` variables in `setup.sh` so that your `$HOME` directory doesn't get filled with cache files.

Write a text file, *e.g.* `list_apps`, of this form:

```
docker://ubuntu:18.04
- ls
- pwd
```

with image addresses, followed by a dashed list of commands you will need to run from that image. Lines starting with `#` will be ignored.

Finally run the script:

```
./setup.sh list_apps
```

At the end of the setup, the script will advise on a couple of variable definitions to be added in your `~/.bash_profile`, something like:

```
For proper functioning of this setup, ensure these two definitions are in your ~/.bash_profile or ~/.bashrc file:

##############################
export PATH=$PATH:[..]
export SINGULARITY_BINDPATH=$SINGULARITY_BINDPATH,[..]
##############################
```

Then, source the file with 
`source ~/.bash_profile` or `source ~/.bashrc`


## Current known limitations

The script does not setup modules, so maintaining multiple version of the same package can be challenging.


## Useful resources

Checkout this tool to install containerised applications, including bash wrappers and modules: [Quay Containers](https://github.com/alexiswl/quay_containers).

