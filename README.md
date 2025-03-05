# O-RAN xApps Closed-Loop Control Using DRL-Based RAN Slicing

## Team Members
- *Soham Sitapara* (202251131)
- *Sudhanshu Bharti* (202251134)
- *Vasani Om* (202251154)
- *Ishan Raj* (202251174)

## Project Description
This project explores the implementation and analysis of *Open Radio Access Network (O-RAN)* using the *ns-O-RAN simulation platform. The focus is on integrating **Near-RT RIC* and *Non-RT RIC* components with custom *xApps* to enable closed-loop control for 5G networks. The study employs *mmWave technology* and *E2 termination* to achieve real-time communication between RAN components, emphasizing:

- *Network optimization*
- *Resource management*
- *Low latency and high reliability*

The project demonstrates the practical viability of O-RAN architecture and highlights its advantages over traditional RAN solutions, such as:

- *Vendor neutrality*
- *Modularity and flexibility*
- *Integration of AI/ML solutions for dynamic resource allocation*

## Key Components
- *ns-3 Integration: Utilizes the **ns3-mmWave* version and *ns-O-RAN module* for detailed 5G simulations.
- *E2 Interface*: Supports real-time communication between RIC and RAN components.
- *RIC and xApp Setup*: Implements xApps for control logic and performance optimization.
- *Closed-Loop Control*: Simulates traffic steering and mobility management using E2 Service Models.

## Hardware and Software Description

### Simulation Environment
- *e2term Terminal*: Logs E2AP interactions for diagnostics and monitoring.
- *xApp Terminal*: Executes custom xApps for dynamic optimization.
- *ns-o-ran Terminal*: Integrates ns-3 with RIC components for seamless simulation.

### System Configuration
- *Central eNB*: LTE-based central node.
- *Four gNBs*: Positioned at 1000m apart, connected via the ns-3 environment.

## Step-by-Step Setup Guide

### 1. Near-RT RIC Setup
This part of the tutorial requires a working version of *Docker* for hosting the RIC on your localhost.

#### Step 1.1: Clone and Setup the Colosseum's Near-RT RIC
bash
git clone -b ns-o-ran https://github.com/wineslab/colosseum-near-rt-ric
cd colosseum-near-rt-ric/setup-scripts


#### Step 1.2: Import Docker Images and Launch RIC
bash
./import-wines-images.sh  # import and tag
./setup-ric-bronze.sh  # setup and launch


#### Step 1.3: Verify RIC Setup
Check if the main entities of the RIC are up and running using:
bash
docker ps


#### Step 1.4: Logging and xApp Setup
Open two terminals for the RIC:

- *Terminal 1*: Logs the E2Term
bash
docker logs e2term -f --since=1s 2>&1 | grep gnb:


- *Terminal 2*: Builds and runs the xApp container
bash
cd colosseum-near-rt-ric/setup-scripts
./start-xapp-ns-o-ran.sh


#### Step 1.5: Run the xApp Logic
Inside the Docker container, navigate to the xApp directory and run the xApp:
bash
cd /home/sample-xapp
./run_xapp.sh


### 2. ns-O-RAN Setup

#### Step 2.1: Install Prerequisites
In *Ubuntu 20.04 LTS*, install the necessary packages:
bash
sudo apt-get update
# Requirements for e2sim
sudo apt-get install -y build-essential git cmake libsctp-dev autoconf automake libtool bison flex libboost-all-dev 
# Requirements for ns-3
sudo apt-get install g++ python3


#### Step 2.2: Clone and Install e2Sim Software
bash
git clone https://github.com/wineslab/ns-o-ran-e2-sim oran-e2sim
cd oran-e2sim/e2sim/
mkdir build
./build_e2sim.sh 3


#### Step 2.3: Clone and Install ns3-mmWave Project
bash
git clone https://github.com/wineslab/ns-o-ran-ns3-mmwave ns-3-mmwave-oran
cd ns-3-mmwave-oran


#### Step 2.4: Clone ns-O-RAN Module
bash
cd ns-3-mmwave-oran/contrib
git clone -b master https://github.com/o-ran-sc/sim-ns3-o-ran-e2 oran-interface
cd ..  # go back to the ns-3-mmwave-oran folder


#### Step 2.5: Configure and Build ns-3
bash
./waf configure --enable-examples --enable-tests
./waf build


### 3. Running the Simulation

#### Step 3.1: Run Scenario Zero
bash
./waf --run scratch/scenario-zero.cc


#### Step 3.2: Verify Message Flow
Ensure the following messages flow between ns-3 and the RIC:
1. *E2 Setup Request* (ns-O-RAN to E2 Term on RIC)
2. *E2 Setup Response* (E2 Term on RIC to ns-O-RAN)
3. *E2 Subscription Request* (xApp to ns-O-RAN through E2 Term on RIC)
4. *E2 Subscription Response* (ns-O-RAN to xApp through E2 Term on RIC)
5. *E2SM RIC Indication Message* (ns-O-RAN to xApp through E2 Term on RIC)

## Contributions and Acknowledgment
We acknowledge the guidance and support of *Dr. Bhupendra Kumar* and the resources provided by *IIIT Vadodara*, which were instrumental in the success of this project.

## References
- *ns-O-RAN Paper*: [Programmable and Customized Intelligence for Traffic Steering in 5G Networks Using Open RAN Architectures](https://arxiv.org/abs/2209.14171)
- *GitHub Repositories*:
  - [ns-O-RAN](https://github.com/wineslab/ns-o-ran)
  - [Colosseum Near-RT RIC](https://github.com/wineslab/colosseum-near-rt-ric)
