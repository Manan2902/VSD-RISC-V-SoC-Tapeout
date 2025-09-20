# VSD-RISC-V-SoC-Tapeout

<details>
  <summary>Week 0</summary>

  # Hardware Design Environment Setup

  A streamlined guide to install and configure essential open-source tools for hardware design using Ubuntu within a VirtualBox environment.

  ## System Requirements

  - **Minimum:** 6GB RAM, 50GB HDD, Ubuntu 20.04+, 4 vCPUs
  - **Recommended:** 10GB RAM, 100GB HDD, 8 vCPUs

  ## Tools and Installation

  ### 1. Yosys

  - [Official Documentation](https://yosyshq.readthedocs.io/en/latest/)
  - Install commands:
    ```
    sudo apt-get update
    git clone https://github.com/YosysHQ/yosys.git
    cd yosys
    sudo apt install make
    sudo apt-get install build-essential clang bison flex libreadline-dev gawk tcl-dev libffi-dev git graphviz xdot pkg-config python3 libboost-system-dev libboost-python-dev libboost-filesystem-dev zlib1g-dev
    make config-gcc
    make
    sudo make install
    ```

  ### 2. Icarus Verilog (iverilog)

  - [Official Documentation](https://steveicarus.github.io/iverilog/)
  - Install command:
    ```
    sudo apt-get update
    sudo apt-get install iverilog
    ```

  ### 3. GTKWave

  - [Official Documentation](https://gtkwave.sourceforge.net/gtkwave.pdf)
  - Install command:
    ```
    sudo apt-get update
    sudo apt install gtkwave
    ```

  ### 4. OpenSTA

  - [Official Documentation](https://github.com/The-OpenROAD-Project/OpenSTA?tab=readme-ov-file)
  - Install required packages:
    ```
    sudo apt update
    sudo apt install -y build-essential cmake clang gcc tcl-dev libffi-dev git flex bison libeigen3-dev swig autoconf libtool libz-dev tcl-dev
    ```
  - Build CUDD:
    ```
    tar xvfz cudd-3.0.0.tar.gz
    cd cudd-3.0.0
    ./configure
    make
    ```
  - Clone and build OpenSTA:
    ```
    git clone https://github.com/parallaxsw/OpenSTA.git
    cd OpenSTA
    mkdir build
    cd build
    cmake -DCUDD_DIR=<CUDD_INSTALL_DIR> ..
    make
    ```

  ### 5. ngspice

  - [Official Documentation](https://ngspice.sourceforge.io/docs.html)
  - Download and unpack tarball, then build:
    ```
    tar -zxvf ngspice-45.2.tar.gz
    cd ngspice-37
    mkdir release
    cd release
    ../configure --with-x --with-readline=yes --disable-debug
    make
    sudo make install
    ```

  ### 6. Magic VLSI

  - [Official Documentation](http://opencircuitdesign.com/magic/)
  - Install dependencies and build:
    ```
    sudo apt-get install m4 tcsh csh libx11-dev tcl-dev tk-dev libcairo2-dev mesa-common-dev libglu1-mesa-dev libncurses-dev
    git clone https://github.com/RTimothyEdwards/magic
    cd magic
    ./configure
    make
    sudo make install
    ```

  ### 7. OpenLane

  - [Official Documentation](https://openlane.readthedocs.io/en/latest/#)
  - Install required packages and Docker:
    ```
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt install -y build-essential python3 python3-venv python3-pip make git
    sudo apt-get remove docker docker-engine docker.io containerd runc
    sudo apt-get install ca-certificates curl gnupg lsb-release
    sudo mkdir -p /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    sudo apt-get update
    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
    sudo docker run hello-world
    sudo groupadd docker
    sudo usermod -aG docker $USER
    sudo reboot # Reboot required
    ```
  - After reboot, verify Docker installation:
    ```
    docker run hello-world
    ```
  - Download and build OpenLane:
    ```
    git clone --depth 1 https://github.com/The-OpenROAD-Project/OpenLane.git
    cd OpenLane/
    make
    make test
    # Optional: view test outputs
    make mount
    ```

  ## License

  Refer to individual tool documentation for licensing details.

</details>
