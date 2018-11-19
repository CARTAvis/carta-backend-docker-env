# Dockerfiles for CARTA

This repository contains Dockerfiles to prepare a build environment for the ASIAA carta-backend. 
Hopefully it would be useful if, for example, you only had a Mac and wanted to test if your carta-backend branch worked in CentOS7/RedHat7.

# Basic instructions

1. Have [Docker](https://www.docker.com) installed and running on your system

2. Clone this repository to your local system 
```
git clone https://github.com/CARTAvis/carta-backend-docker-env.git
```

3. To build the CentOS7 version, issue the following command and wait for it to build (It may take ~30 minutes or longer)
```
docker build -f Dockerfile-centos7 -t asiaa-carta-backend-centos7 .
``` 

4. Run the container 
```
docker run -p 3002:3002 -v ~/CARTA:/root/CARTA -ti asiaa-carta-backend-centos7
```
Note the -v flag can mount any local directory inside the container. Here we want to mount your local CARTA directory (typically at ~/CARTA) so that CARTA 
will have access to your Images and cache directories.

5. You will automatically be in the /cartawork directory, and can build CARTA in the usual way, e.g. 
```
git clone https://github.com/CARTAvis/carta-backend.git 
cd carta-backend
git submodule init
git submodule update
mkdir build
cd build
qmake NOSERVER=1 CARTA_BUILD_TYPE=dev ../carta -r
make -j 4
./cpp/desktop/CARTA
```

6. At this point CARTA should be running with the default port 3002, waiting to connect to a carta-frontend running on your local system that is 
listening for port 3002 (Of course, you can change the port number in the Dockerfile and docker run command if you wish), and start CARTA specifying the different port e.g. ```./CARTA --port=5000``` and similarly modyifing the hidden ```.env``` file in the carta-frontend to listen to the same port e.g. ```REACT_APP_DEFAULT_ADDRESS=ws://localhost:5000```

---
Similarly, for the Ubuntu version,
build:
```
docker build -f Dockerfile-ubuntu -t asiaa-carta-backend-ubuntu .
```
and run:
```
docker run -p 3002:3002 -v ~/CARTA:/root/CARTA -ti asiaa-carta-backend-ubuntu
```
---

Note: These are just basic instructions. Feel free to study and modify the Dockerfile if, for example, you want to use a different Qt version (in this case it uses Qt 5.9.4), test different library versions, mount your own local carta-backend directory, or incorporate the CARTA build steps as a script etc.


