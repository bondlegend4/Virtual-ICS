# ICSs virtualized lab for cybersecurity testing

## Description

- This project is currently under development and is supported by [@sfl0r3nz05](sfigueroa@ceit.es). The aim of this project is to develop the deployment of an ICS network on which to perform security tests.

- The specific use case to be implemented is that of a *Waster Water Treatment Plant (WWTP)*. However the project may integrate other use cases such as the [Tennesse-Eastman](http://depts.washington.edu/control/LARRY/TE/download.html).

- This project is related to the [OT-NWbasedOnGNS3](https://github.com/sfl0r3nz05/OT-NWbasedOnGNS3) project, aiming to deploy the use case of this project on a larger ICS network.

- The project consists of four container-based components:
  1. [OpenPLC](https://github.com/thiagoralves/OpenPLC_v3)
  2. [Scada-LTS](https://github.com/SCADA-LTS/Scada-LTS)
  3. [ICS Process]()
  4. [Attacker]()

- These components are deployed in two ways:
  1. [Docker compose network]()
  2. [GNS3 network simulation]()

**Project Status:** `in progress`

## State of the Art

This is a compilation of works related to the theme of the project:

1. [Development of an Open-Source Testbed Based on the Modbus Protocol for Cybersecurity Analysis of Nuclear Power Plants](./StateofArt/applsci-12-07942.pdf)

## Requirements

- For deployment based on `docker compose`:

  1. Install [docker for ubuntu](https://docs.docker.com/engine/install/ubuntu/).
  2. [Manage Docker as non-root user](https://docs.docker.com/engine/install/linux-postinstall/).
     1. If it does not work try running `sudo chmod o+rw /var/run/docker.sock` after adding a new user
  3. Install [docker-compose for ubuntu](https://docs.docker.com/compose/install/).
  4. Install make `sudo apt install make`.
  5. Install g++:
     1. `sudo apt-get update`
     2. `sudo apt-get install -y g++`

- For deployment based on `GNS3`:

  > **Note**: For deployment over GNS3, the same previous requirements should be used.

  1. [GNS3 server installation](./documentation/Requirements/gns3.md)

## Build and set up the component containers

This section explains how to build and set-up the containers that will be imported in both the `docker compose` and `GNS3`.

1. [OpenPLC container creation](./documentation/Components/OpenPLC.md)

2. ICS Process containers:

    > **Note:** The following containers will be associated with the industrial processes to be integrated in the project

    1. [WWTP Process container based on Matlab](./documentation/Components/Matlab.md)

3. [Scada-LTS container creation](./documentation/Components/Scada-LTS.md)
4. [Attacker container creation](./documentation/Components/Attacker.md)

### Getting Started network ICSNetwork

<details>

<summary>Open to see details</summary>

- Add permissions
  - `cd ~/ICSVirtual/network/ICSNetwork`
  - `sudo chmod +x scripts/*.*`

</details>

### Getting Started TCPDump

<details>

<summary>Open to see details</summary>

- To capture the traffic into the ICSNetwork the [TCPDump](https://www.tcpdump.org/) tool is used.
- To deploy as part of the Docker Infrastructure `kaazing/tcpdump` image is [used](https://hub.docker.com/r/kaazing/tcpdump).
- Once the `tcpdump` container is deployed an `*.pcap` file is included as part of the `tcpdump` folder.

  ![alt text](./images/tcpdump1.png)

- Once `*.pcap` file is downloaded, it can be opened using Wireshark.
  
  ![alt text](./images/tcpdump2.png)
  
</details>

## How to use the project

### Deploy ICSNetwork

- `cd ~/ICSVirtual/network/ICSNetwork`

- `make start`

- `make stop`

- `make destroy`

### Deploy tcpdump

- `cd ~/ICSVirtual/network/tcpdump`

- `make start`

- `make stop`

- `make destroy`

### Deploy Attacker

#### Single Attacker

- `cd ~/ICSVirtual/network/attacker`

- `make start`

- `make stop`

- `make destroy`

#### ModTester

- `cd ~/ICSVirtual/network/modtester`

- `make start`

- `docker exec -it <modtester-container-id> bash`
  - E.g.: `docker exec -it 91d48b6bdabd bash`
  
- Inside the container execute:
  
  - `python modTester.py`
  
  - `show modules` / `use module_name`
    - E.g.: `use modbus/dos/floodingAttack`

  - `show options` to see options to complete.
    - E.g.: `set RHOSTS ip`  -->  `set RHOSTS 172.18.0.2`
    - E.g.: `set sIP ip`  -->  `set sIP 172.18.0.3`
  
  - `exploit`

- `make stop`

- `make destroy`

## How to test connection

- `docker inspect <containerid>`
  
- `docker exec -it <containerid> bash`
  
### Install/use ping network tool

- `sudo apt-get update`

- `sudo apt-get install iputils-ping`
  
- e.g.: `ping 192.168.144.3`
