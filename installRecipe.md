# Installation Recipes
## Method-1: Installing GROMACS on Ubuntu with CUDA GPU Support
### Getting Started

Update your repository information and software packages before
`$ sudo apt-get update`
`$ sudo apt-get upgrade`

### Downloading CUDA toolkit

If you have already NVIDIA CUDA drivers installed on your system. 
First remove them and then install the latest drivers, otherwise, it will give you errors. Open a terminal and type the following commands:

`$ sudo apt-get remove --purge cuda-* libcuda* nvidia*`
not necessarly the nvidia drivers 

`$ sudo apt-get remove --purge cuda-drivers libcuda* cuda-runtime* cuda-8-0 cuda-demo*`

`$ sudo apt-get autoremove --purge nvidia* cuda-drivers libcuda* cuda-runtime* cuda-8-0 cuda-demo*`

`$ sudo apt-get remove --purge nvidia* cuda-drivers libcuda1-396 cuda-runtime-9-2 cuda-9.2 cuda-demo-suite-9-2 cuda`

Now select the latest version of the CUDA toolkit according to your system from [here](https://developer.nvidia.com/cuda-downloads). Download the local run file.

`$ cd Downloads/`
`$ wget https://developer.download.nvidia.com/compute/cuda/11.1.1/local_installers/cuda_11.1.1_455.32.00_linux.run`

### Installing CUDA toolkit

`$ sudo sh cuda_11.1.1_455.32.00_linux.run`

Edit _/etc/ld.so.conf_ file.

`$ sudo gedit /etc/ld.so.conf`

Now add the following path at the end of the file: `/usr/local/cuda-11.1/lib64`

`$ sudo ldconfig`
  
  
ONLY if not previously installed
>   Install CUDA drivers. To avoid any error, don’t forget to write the
> last argument in the command (_–override-driver-check_).
> 
> `$ sudo sh cuda_11.1.1_455.32.00_linux.run --silent --driver
> --override-driver-check`

### Updating NVIDIA drivers

It is necessary to update the NVIDIA drivers.

`$ sudo ubuntu-drivers devices`

Check versions and recommendations, *autoinstall* gets the recommended.

`$ sudo ubuntu-drivers autoinstall`
`$ sudo apt-get update`

### Installing prerequisites for GROMACS

-   Install openmpi from the repository using the following command:

`$ sudo apt-get install libopenmpi-dev`
`$ sudo apt-get install openmpi-bin`
`$ sudo apt-get install mpi4py`

-   Get cmake before installing GROMACS. In the terminal, type:

`$ sudo apt-get install -y cmake`

Check the version of cmake by the following command:

`$ cmake --version`

-   The next requirement is build-essential:

`$ sudo apt-get install -y build-essential`

> If not using GROMACS own libraries.
> -   Now, you need the FFTW3 library. Install it using the following command: `$ sudo apt-get install -y libfftw3-dev`

-   Now, install Doxygen

`$ sudo apt-get install -y doxygen`

### Downloading Regressiontests

It is better to download the regressiontests separately because most of the time it throws an error stating that “the location of the file has changed”. Copy and paste the following commands in your terminal:

`$ cd Downloads/`

`$ wget http://gerrit.gromacs.org/download/regressiontests-2020.4.tar.gz`

Now, extract the downloaded file,

`$ tar xvzf regressiontests-2020.4.tar.gz`

### Downloading GROMACS

Download the latest release of GROMACS from [here](http://manual.gromacs.org/documentation/) or use the following command:

`$ wget ftp://ftp.gromacs.org/pub/gromacs/gromacs-2020.4.tar.gz`

### Installing GROMACS

First, get your _pwd_ path using the following command:

`$ pwd`

Note down the displayed path, we will need it in later steps.

-   Extract the downloaded archive file. Move into the directory where you have downloaded the package. Let’s say, _Downloads_.

`$ cd Downloads/`

`$ tar xvzf gromacs-2020.4.tar.gz`

-   Now move inside the gromacs folder.

`$ cd gromacs-2020.4/`

-   Create a directory called “_build-gpu_” where we will keep our compiled binaries.

`$ mkdir build-gpu`

-   Move inside the _build-gpu_ directory

`$ cd build-gpu/`

-   Now, make gromacs using _cmake._

`$ sudo cmake .. -DGMX_BUILD_OWN_FFTW=ON -DMAKE_C_COMPILER=gcc -DGMX_GPU=ON -DGMX_MPI=OFF -DREGRESSIONTEST_DOWNLOAD=OFF -DREGRESSIONTEST_PATH=/your/pwd/path/here/Downloads/regressiontests-2020.4`

If everything goes well, the message in your terminal will say “Generating Done. Build files written… “. If not, make sure you have replaced the pwd path in command with the path of your home directory.

-   Now, let’s check and make.

`$ make check`

`$ sudo make install`

-   Move inside the unpacked regressiontests directory and execute the following command:

`$ source /usr/local/gromacs/bin/GMXRC`

After that, you can check for the successful installation of GROMACS with the following command:

`$ gmx pdb2gmx --version`

It will display the GPU version as well.

Enjoy the speedy simulation!


> Written with [StackEdit](https://stackedit.io/).
