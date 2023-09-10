# SBPL-for-ARM-processor
The SBPL and sbpl_lattice_planner algorithms works well in x86 processors but not on ARM based processors. To rectify this issue ,the build configs need to be modified.

### INSTALLATION
    cd <Your ros workspace>/src
    git clone https://github.com/nsk-05/SBPL-for-ARM-processor.git
    cd sbpl
    mkdir build && cd build
    cmake ..
    make
#### Install the sbpl library in local system
    sudo make install 

### BUILD as ros package
    cd <Your ros workspace>
    catkin_make
