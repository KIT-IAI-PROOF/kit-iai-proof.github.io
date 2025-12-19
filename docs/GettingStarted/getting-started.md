# Prerequisites

To run PROOF in different environments, ensure you have the following prerequisites installed:

### Execution with Docker
The Docker environment (Docker engine) must be installed on your host system. 

To install docker and to see more information about docker, please follow the instructions on the docker homepage: [www.docker.com](https://www.docker.com/). Alternatively, you can install docker by using the command line and the package sources for your corresponding operations system distribution.

**Windows:** Follow the official [docker documentation](https://docs.docker.com/desktop/setup/install/windows-install/#install-docker-desktop-on-windows).

<br>
All other required components are provided via Docker images.

**Exception:** If you want to create your own *Blocks*, you need to have Python, Java, or MATLAB installed on your host system to create and test the model programs.

<!--
### Local Execution
will be documented in the next release (2026)

### Execution with Kubernetes
will be documented in the next release (2026)
-->
<br>

# Installation
Follow these steps to start and run PROOF.

## Get proof-environment
To install PROOF, move to the repository [proof-environment](https://github.com/KIT-IAI-PROOF/proof-environment) and clone the repository to your local file system. We recommend creating a PROOF directory for this purpose. To clone the repository using a cli, make sure you have **git** installed on your local machine. Alternatively, you can download the zip-file of proof-environment.

## Download the worker image
The docker image for the (python) worker needs to be downloaded manually using the following command in a terminal/command line:
```
docker pull ghcr.io/kit-iai-proof/proof-worker-python:1.0.0
```

## Start PROOF
To start PROOF, navigate to the following folder within proof-environment:
```
cd proof-environment/docker
```
Afterwards, start PROOF using the compose file in the docker folder:
```
docker compose up
```
**Windows:** Ensure to have the **Docker Desktop App** running.

PROOF will take a few moments to be started properly. Thus, you can access the PROOF graphical user interface by opening a new browser window/tab and type the following in the URL address bar:
```
localhost:80
```
Due to caching behaviour in some browsers, we **strongly recommend** opening the PROOF UI in a **private browser window** at this moment.

To access the workflow creation editor, the credentials to be used are *test/test*.

## Data exchange
Data exchange with PROOF currently works via the `proof-environment/data` folder. All files laying in this folder can be accessed e.g. in the *FileLineProvider* block to be read from.

# Test your installation
To test your installation, you can use the workflow *FileReader2FileWriter*. It contains two blocks: the *FileReader* which reads a file given as input parameter and provides the next line each step and the *FileWriter* which receives data and appends it to the file which name is given as an input parameter.

When you see in the Orchestrator logs that all blocks are shutdown, can can check the contents of the output file (e.g. *out.txt*) which should be the same as the *input.txt* file.
```
cd proof-environment/data
cat out.txt
```