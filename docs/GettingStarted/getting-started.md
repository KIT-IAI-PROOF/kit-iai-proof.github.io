# Prerequisites

To run PROOF in different environments, ensure you have the following prerequisites installed:

### Execution with Docker
The Docker environment (Docker engine) must be installed on your host system. 

To install docker and to see more information about docker, please follow the instructions on the docker homepage: [www.docker.com](https://www.docker.com/). Alternatively, you can install docker by using the command line and the package sources for your corresponding operations system distribution.

**Windows:** Follow the official [docker documentation](https://docs.docker.com/desktop/setup/install/windows-install/#install-docker-desktop-on-windows).

<br>
All other required components are provided via Docker images.

**Exception:** If you want to create your own *Templates*, you need to have Python, Java, or MATLAB installed on your host system to create and test the model programs.

<!--
### Local Execution
will be documented in the next release (2026)

### Execution with Kubernetes
will be documented in the next release (2026)
-->

# Installation
Follow these steps to start and run PROOF.

## Get proof-environment
To install PROOF, move to the repository [proof-environment](https://github.com/KIT-IAI-PROOF/proof-environment) and clone the repository to your local file system. We recommend creating a PROOF directory for this purpose. To clone the repository using a cli, make sure you have **git** installed on your local machine. Alternatively, you can download the zip-file of proof-environment.

## Download the worker image (optional)
Starting with version 1.1.0, PROOF automatically downloads missing Docker images when needed. However, you can optionally download the [proof-worker-python image](https://github.com/orgs/KIT-IAI-PROOF/packages/container/package/proof-worker-python) in advance to speed up the first execution:
```
docker pull ghcr.io/kit-iai-proof/proof-worker-python:1.2.0
```

## Start PROOF
To start PROOF, navigate to proof-environment and start PROOF using the provided startPROOF scripts:  
**Linux / Mac:**
```
./startPROOF
```
**Windows:**  

- Ensure to have the **Docker Desktop App** running.  
- Start PROOF using the startPROOF.bat file.

PROOF will take a few moments to be started properly. Thus, you can access the PROOF graphical user interface by opening a new browser window/tab and type the following in the URL address bar:
```
localhost
```
Due to caching behaviour in some browsers, we **strongly recommend** opening the PROOF UI in a **private browser window** at this moment.

To access the workflow creation editor, the **credentials** to be used are *proof/proof*.

## Stop PROOF
Don't forget to stop PROOF before shutting down. To ensure network instabilities do not prevent PROOF from running, PROOF comes with a restart policy for the docker containers if not stopped.  
Therefore simply use the stopPROOF scripts:  
**Linux / Mac:**
```
./stopPROOF
```
**Windows:**  
 
- Stop PROOF using the stopPROOF.bat file.

## Standard Ports
PROOF is using certain port numbers per default and therefore expects them not to be allocated at the time of the start. These ports are:
- proof-keycloak: 8080
- proof-postgres: 5432
- proof-redis: 6379
- proof-config-manager: 8100
- proof-orchestrator: 8200
- proof-frontend: 80
- proof-rabbitmq: 5672, 15672, 15674, 61613

In case these port need to be changed (e.g. one of them is already in use), they can be adjusted in the docker-compose.prod.yaml file under proof-environment/docker. \
In case it is unclear why rabbitmq requires multiple ports, see the rabbitmq documentation [here](https://www.rabbitmq.com/docs/networking). 

## Data exchange
Data exchange with PROOF works via the `proof-environment/data` folder structure:

- **`userdata/`**: Directory for user-managed files that can be accessed by workflow blocks (e.g., *FileReader* block)
- **`attachments/`**: Directory for workflow-specific attachments and configuration files
    - Uploaded attachment files are stored here, organized by their unique attachment ID.
- **`executions/`**: Directory for execution-specific data and logs
    - Model file logs are stored in subdirectories named by execution ID as specified in the docker-compose file.

Files placed in the `userdata` folder can be referenced in your workflows for reading and writing operations.

## Test your installation
To test your installation, you can use the workflow *FileReader-to-FileWriter*. It contains two blocks:
- the *FileReader* which reads a file given as input parameter and provides the next line each step
- the *FileWriter* which receives data and appends it to the file which name is given as an input parameter.

To start the workflow you can use the `input.txt` as input file for the *FileReader*. It is already available in the `userdata` folder.  
As output file for the *FileWriter* simply use a custom file name e.g. *out.txt*. The output file will be stored in the `userdata` directory.

When you see that the execution has the status SHUT_DOWN, you can check the contents of the output file (e.g. *out.txt*) which should be the same as the input file for this demo workflow. This can e.g. be done via console:
```
cd proof-environment/data/userdata
cat out.txt
```