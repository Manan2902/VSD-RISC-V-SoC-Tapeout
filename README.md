# VSD-RISC-V-SoC-Tapeout

<details>
  <summary><h2>Week 0</h2></summary>

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

Day 1
  
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


Day 2

Introduction to .lib Library Files

When examining a .lib (library) file, three key parameters must be checked:

    P (Process): Captures process-related variations during fabrication.

    V (Voltage): Captures supply voltage variations.

    T (Temperature): Captures operational temperature variations.

Hierarchical vs. Flat Synthesis

    Hierarchical Synthesis: Useful for designs with repeated modules, enabling a divide-and-conquer strategy—commonly applied in large-scale projects.

    Flat Synthesis: Combines all hierarchy into a single-level netlist, which can simplify optimization across the design.

Flattening a design in Yosys:

bash
flatten
write_verilog -noattr multiple_modules_flat.v

Design Considerations

    Avoid stacked PMOS transistors in certain design practices.

    Explore concepts such as logical effort for timing optimization.

Module-Level Synthesis

    Recommended when a design includes multiple instances of the same module.

    Supports divide-and-conquer strategies for massive projects.

Flip-Flop Coding Styles and Optimization

    Be mindful of glitches in flip-flops.

    Distinguish between synchronous and asynchronous (sync/async) resets in flip-flop design.



Day 3

Introduction to Optimizations

This section covers practical techniques to optimize both combinational and sequential logic in digital designs.
Combinational Logic Optimization

    The goal is to reduce area and power while maximizing performance.

    Typical methods include:

        Constant propagation: Replacing parts of the circuit with constant values where possible.

        Direct optimization: Simplifying logic expressions.

        Boolean logic optimization: Using algebraic methods to reduce gate count and complexity.

Sequential Logic Optimization

    Focuses on refining circuits that store and manage state.

    Optimization strategies include:

        Sequential constant propagation: Propagating known values through sequential elements.

        State optimization: Minimizing the number of states in state machines.

        Retiming: Moving registers across logic to improve timing without changing behavior.

        Sequential logic cloning (floorplan-aware synthesis): Duplicating logic where advantageous for parallelism and timing.



</details>

    


</details>

<details> <summary><h2>Week 2</h2></summary>

## Sources

The following content is compiled from these sources:

*   [Ansys Blog: What is a System on a Chip (SoC)?](https://www.ansys.com/en-gb/blog/what-is-system-on-a-chip)
    
*   [Synopsys Blog: System-on-Chip Design](https://www.synopsys.com/blogs/chip-design/system-on-chip-design.html)
    
*   [Synopsys Blog: System on Chip](https://www.synopsys.com/blogs/chip-design/system-on-chip.html)
    

* * *

# System on Chip (SoC): Comprehensive Guide

In electronics, the name of the game is "more performance, less power, and less space." Especially in portable devices such as tablets and smartphones, massively complex technology must fit within the smallest possible footprint and use the least amount of power. To create devices that are both fast and small, engineers consolidate all necessary components into a single package, called a system on a chip (SoC).

## What is a System on Chip?

A system on a chip is an integrated circuit that compresses all of a system's required components onto one piece of silicon. By eliminating separate and large system components, SoCs simplify circuit board design, resulting in improved power and speed without compromising system functionality. Generally, SoCs include:

*   **Multiple Cores**: Processors in the form of microcontrollers, microprocessors, digital signal processors, or application-specific instruction set processors
    
*   **Memory Capabilities**: RAM, ROM, FLASH, EEPROM, and/or cache memory
    
*   **External Interfaces**: Wired communication protocols such as HDMI, USB4, FireWire, USART, SPI, I²C, or Ethernet
    
*   **Wireless Capabilities**: WiFi, Bluetooth, and other radio frequency capabilities
    
*   **GPU**: Graphical Processing Unit for accelerating specific tasks
    
*   **Voltage Regulation**: Voltage regulators, phase lock loop (PLL) control systems, built-in oscillators, timers, and analog-to-digital (ADC) converters
    
*   **Intrachip Communication**: Interface busses or networks-on-chip (NoC) for connecting individual circuit blocks
    
*   **Signal Processing**: Digital, analog, and mixed-signal processing circuit blocks
    

Compact SoCs have become indispensable solutions spanning from wired applications like data centers, artificial intelligence (AI), and high-performance computing (HPC) to battery-operated devices like mobile phones and wearables.

## SoC Design: Pros and Cons

## Advantages

*   **Space optimization**: Smaller device designs possible
    
*   **Power efficiency**: Significant reduction in power consumption
    
*   **Cost-effective**: Single SoC cheaper than multiple separate chips
    
*   **Reliability**: Fewer connections increase system reliability
    
*   **Performance**: On-chip signals achieve higher performance and speed
    

## Disadvantages

*   **Single point of failure**: Component failure affects entire system and limits upgrades
    
*   **Time to market**: Custom SoCs require specialized expertise, tools, and increased development time
    
*   **Mixed analog/digital**: Single process technology limits analog performance optimization
    
*   **Flexibility**: Limited scope for applications beyond intended task
    

## System on a Chip Design Flow

The SoC design workflow involves several collaborative stages:

1.  **Specification**: Define desired function, applications, performance goals, and power limitations
    
2.  **Logical design**: Describe behavior in hardware description language (HDL) and simulate functionality
    
3.  **Logic synthesis**: Translate HDL description into transistor elements and interconnections (netlist)
    
4.  **Physical design**: Determine transistor locations and interconnection wire trajectories
    
5.  **Signoff**: Analyze and validate design using verification software to ensure functionality and manufacturability
    
6.  **Tapeout**: Generate final graphic files for photomasks and send to manufacturer
    
7.  **Testing and packaging**: Confirm specifications and encapsulate in protective package
    

## Integration Within SoC Architecture

## Processor Cores

SoCs typically contain multiple processor cores using RISC instruction set architecture for reduced digital logic, power consumption, and area. ARM architectures are commonly used as available IP cores.

## Memory Blocks

Critical memory components include ROM, RAM, EEPROM, and flash memory. Cache hierarchies use SRAM for processor registers and DRAM for main memory.

## External Interfaces

Communication protocols include USB, FireWire, USART, SPI, Ethernet, HDMI, I2C, and wireless networking protocols like Bluetooth, Wi-Fi, and RFID.

## Supporting Circuitry

Essential functionality components include voltage regulators, power management circuits, phase-locked loop control systems, clocks, timers, oscillators, and analog-to-digital converters.

## SoC Inter-Module Communication Designs

Traditional data bus architectures like ARM's Advanced Microcontroller Bus Architecture (AMBA) have limited scalability, supporting only tens of cores. Wire delay scaling issues and increased power consumption have led to network-on-chip (NoC) technology adoption.

NoC technology offers application-specific routing, improved power efficiency, and reduced bus contention. Modern NoC architectures utilize distributed computing network topologies such as torus, hypercube, mesh, and tree networks to efficiently meet SoC power and throughput requirements.

1.  [https://www.ansys.com/en-gb/blog/what-is-system-on-a-chip](https://www.ansys.com/en-gb/blog/what-is-system-on-a-chip)

</details>
