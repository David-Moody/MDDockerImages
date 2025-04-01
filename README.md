# Molecular Dynamics Docker Images

## Common requirements

### Install Docker or equivalent container system

For example:

Docker Engine for Debian: <https://docs.docker.com/engine/install/debian/>

Or Docker Desktop for Windows: <https://docs.docker.com/desktop/setup/install/windows-install/>

### Install Nvidia Container Toolkit (for GPU access)

Docker Desktop for Windows already has the required software. Other than ensuring your GPU drivers are up to date, nothing else is needed.

NCT for Linux: <https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html>

### Clone repo

    git clone https://github.com/David-Moody/MDDockerImages.git

## Amber

Current version: Amber24

### Download required source files

Download AmberTools24 via Option 2: <https://ambermd.org/GetAmber.php#ambertools>

Download Amber24: <https://ambermd.org/GetAmber.php#amber>

**This has a licensing agreement that you must meet. Including a license fee for commercial use.**

Place Amber24.tar.bz2 and AmberTools24.tar.bz2 into Amber folder.

### Build the container

    cd Amber
    docker build . -t amber-cuda:v1
    # Run the tests
    docker run --gpus=all -it amber-cuda:v1 make test.cuda.serial -j12

### Using the container

Change to the directory with your project files for Amber

    docker run --gpus=all -it --rm -v .:/data amber-cuda:v1 <AMBER COMMAND>

For example:

    docker run --gpus=all -it --rm -v .:/data amber-cuda:v1 cpptraj
OR

    docker run --gpus=all -it --rm -v .:/data amber-cuda:v1 /data/dynamics.sh

## GROMACS

Current version: 2025.1

    cd GROMACS
    docker build . -t gromacs:2025.1

### Running GROMACS

    docker run --gpus=all -it --rm gromacs:2025.1

With the current working directory bind mounted (like a symbolic link) to /data inside the container.

    docker run --gpus=all -it --rm -v .:/data gromacs:2025.1 /data/dynamics.sh

### Options to consider adding

-DGMX_SIMD=xxx to specify the level of SIMD support of the node on which GROMACS will run

## OpenMM

Current version: Always latest

    cd OpenMM
    docker build . -t openmm:latest

### Running OpenMM

    docker run --gpus=all -it --rm -v .:/data openmm:latest /data/dynamics.sh
    docker run --gpus=all -it --rm openmm:latest
