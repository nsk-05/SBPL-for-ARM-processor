# SBPL-for-ARM-processor
The SBPL and sbpl_lattice_planner algorithms works well in x86 processors but not on ARM based processors. To rectify this issue ,the build configs need to be modified.

### SBPL INSTALLATION
    git clone https://github.com/nsk-05/SBPL-for-ARM-processor.git
    cd sbpl
    mkdir build && cd build
    cmake ..
    make
    
#### Install the sbpl library in local system
    sudo make install 

### Mention the path of the locally installed sbpl lib in bashrc 
#### (ref https://stackoverflow.com/questions/67130750/cmake-find-library-not-finding-library-specified-by-paths)
    echo PKG_CONFIG_PATH=/usr/local/lib/pkgconfig:$PKG_CONFIG_PATH >> ~/.bashrc

### In jetson nano, need to specify the processor archeitecture for proper source installation of sbpl_lattic_planner:
#### (ref https://stackoverflow.com/questions/65631801/illegal-instructioncore-dumped-error-on-jetson-nano)
    export OPENBLAS_CORETYPE=ARMV8


### BUILD Sbpl_lattice_planner 
##### Place the sbpl_lattice_planner in your ros workspace and make package 
    cd <Your ros workspace>/
    catkin_make
