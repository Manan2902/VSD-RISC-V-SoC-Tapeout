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
<img width="755" height="213" alt="yosys install" src="https://github.com/user-attachments/assets/e9a8ce0b-0c93-4d31-a0ab-b75c6f5474e0" />



  ### 2. Icarus Verilog (iverilog)

  - [Official Documentation](https://steveicarus.github.io/iverilog/)
  - Install command:
    ```
    sudo apt-get update
    sudo apt-get install iverilog
    ```

<img width="815" height="617" alt="iverilog install" src="https://github.com/user-attachments/assets/aca7dd11-5a22-482e-8fc0-47405d713aa2" />




  ### 3. GTKWave

  - [Official Documentation](https://gtkwave.sourceforge.net/gtkwave.pdf)
  - Install command:
    ```
    sudo apt-get update
    sudo apt install gtkwave
    ```

    
<img width="815" height="105" alt="gtkwave install" src="https://github.com/user-attachments/assets/348f8c0b-f467-4d57-ac3f-f6180e3c9025" />



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

    
<img width="815" height="225" alt="ngspice install" src="https://github.com/user-attachments/assets/126f7829-57cc-441d-9187-21a176ca7f08" />


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

    
<img width="1727" height="915" alt="magic install" src="https://github.com/user-attachments/assets/ab376b42-b369-4297-bce7-d8886c011447" />
    

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
    # Open the spm.gds using KLayout with sky130 PDK
    klayout -e -nn $PDK_ROOT/sky130A/libs.tech/klayout/tech/sky130A.lyt \
     -l $PDK_ROOT/sky130A/libs.tech/klayout/tech/sky130A.lyp \
     ./designs/spm/runs/openlane_test/results/final/gds/spm.gds

    # Leave the Docker  
    exit
    ```
    
<img width="1727" height="915" alt="OpenLane install" src="https://github.com/user-attachments/assets/ac2b4e41-d031-49fe-8573-6f56aac2a3f4" />
</details>

<details> <summary><h2>Week 1</h2></summary>
Introduction

This section presents the foundational concepts used in RTL design and simulation, focusing on Verilog workflows and tool usage.
Simulator

    The RTL design is simulated to check adherence to specifications.

    A simulator is used for this purpose.

    Icarus Verilog (iverilog) is the chosen tool for this workshop.

Design

    Design refers to the Verilog code that implements the intended functionality based on the given specifications.

Testbench

    The testbench applies stimulus (test vectors) to the design, verifying its operation.

How the Simulator Works

    The simulator monitors changes in the input signals.

    When an input changes, outputs are re-evaluated.

    No change in the input means outputs remain unchanged.

Repository Structure and Usage

Clone the official workshop repository:

bash
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git

    The repository includes standard cell libraries and all Verilog modules used in the workshop.

    Inside the verilog_files directory, Verilog modules are paired with corresponding testbench files.

To run an example module:

bash
iverilog good_mux.v tb_good_mux.v
./a.out
gtkwave tb_good_mux.vcd

    Compiling generates a.out.

    Executing ./a.out creates a .vcd file for waveform viewing with GTKWave.
    
<img width="1920" height="1080" alt="2" src="https://github.com/user-attachments/assets/c269bc8b-6174-44e6-ab8c-4d9fdd25299d" />

<img width="1920" height="1080" alt="5" src="https://github.com/user-attachments/assets/d590f99f-866f-4cc4-a51c-b5b1305491d6" />




</details>

    

  ## License

  Refer to individual tool documentation for licensing details.

</details>
