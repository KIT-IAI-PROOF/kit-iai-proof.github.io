## Creating Custom Python Images
Although a base docker image for python is provided, the libraries that are included within the base image may be insufficient for different kinds of scenarios. Therefore, custom images can be created by following these steps:

1. Create a file *requirements.txt* and let it contain all the libraries you wish to include in the custom image in a seperate line.  
requirements.txt:  
```
paho-mqtt
pyro5
```
2. Create your Dockerfile (a file name *Dockerfile*) in a directory of your choice. See that it is in the same directory as your previously created *requirements.txt*.
1. Let your custom docker image extend our base worker python image.  
Dockerfile:
```dockerfile
FROM ghcr.io/kit-iai-proof/proof-worker-python:1.2.0
# requirements.txt must be located in the same directory as this Dockerfile!
COPY requirements.txt requirements.txt
# Install all python packages listed under requirements.txt into the predefined python venv
RUN pip install --no-cache-dir requirements.txt && rm -rf /var/lib/apt/lists/* requirements.txt
```
4. Build the docker image by giving it a name and a tag. The name should give you a hint on the purpose or functionality of your image, while the tag should give you information about the image's current version and/or state. Name and tag are seperated by a colon (:). Use the following command:
```
docker build -t <your-image-name:your-image-tag>
```

  The command has to be executed from within the directory where your *Dockerfile* and *requirements.txt* are located.
  If your are managing multiple Dockerfiles in the same directory or if you are using a Dockerfile with a name different from *Dockerfile*, use the following command:

```
docker build -f <your-dockerfile> -t <your-image-name:your-image-tag>
```
5. Afterwards, you may want to test whether your build and the installation of the libraries were successful. You can do so by starting a container of your image which only gives you feedback about the status of the libraries you wanted to install. For this example, we assume to test the libraries that were stated previously as content of our *requirements.txt*. Use the following command:  
    ```
    docker run --rm --entrypoint /bin/bash <your-image-name>:<your-image-tag> -c "python -m pip show pyro5 paho-mqtt"
    ```

    - If your libraries are installed successfully, you should see an output similar to this one:
```
Name: Pyro5
Version: 5.16
Summary: Remote object communication library, fifth major version
Home-page: https://github.com/irmen/Pyro5
Author: Irmen de Jong
Author-email: irmen@razorvine.net
License: MIT
Location: /python-deps
Requires: serpent
Required-by: 
---
Name: paho-mqtt
Version: 2.1.0
Summary: MQTT version 5.0/3.1.1 client class
Home-page: http://eclipse.org/paho
Author: 
Author-email: Roger Light <roger@atchoo.org>
License: EPL-2.0 OR BSD-3-Clause
Location: /usr/local/lib/python3.12/site-packages
Requires: 
Required-by: 
```

 If installing was not successful, you should see something similar to:
```
WARNING: Package(s) not found: pyro5, paho-mqtt
```

In that case, check whether you spelled the names of your libraries correct in your *requirements.txt* and restart the build from step 4 again. Make also sure to have access to the internet while doing the build, as the corresponding information is fetched during from remote servers.
