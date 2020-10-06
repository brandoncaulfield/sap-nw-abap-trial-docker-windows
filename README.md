# SAP NW ABAP Trial in Docker on Windows

This guide outlines how to install the SAP NW ABAB Developer Trial 7.52 SP04 using Docker on a Windows 10 machine.

## Steps to install SAP ABAP Trial using Docker on Windows
1. Clone this GitHub repo
```sh
git clone https://github.com/brandoncaulfield/sap-nw-abap-trial-docker-windows
```

2. Create a new folder in the local repo folder called **sapdownload**
3. Copy the extracted rar files to the **sapdownload** folder
4.  Open Command Prompt or PowerShell and navigate to your newly cloned repo folder and run the docker build command:
```sh
docker build -t nwabap:7.52 .
```

5. Once the build has finished you need to adjust your **vm.max_map_count=1000000**. From a new Command Prompt or Powershell window run the following commands:
```sh
wsl -d docker-desktop
sysctl -w vm.max_map_count=1000000
```
Check that the vm.max_map_count has actually changed by running this command:
```sh
sysctl vm.max_map_count
```
 
6. Then run your docker container
```sh
docker run -p 8000:8000 -p 44300:44300 -p 3300:3300 -p 3200:3200 -h vhcalnplci --name nwabap752 -it nwabap:7.52 /bin/bash
```

7. Once your container is running you need to begin installing the SAP system. This could take a while so be patient! :)
```sh
/usr/sbin/uuidd
./install.sh
``` 

8. Once the SAP system is installed successfully run the following commands:
```sh
docker start -i nwabap752
/usr/sbin/uuidd
```

9. Once your SAP system is running you can start you need to start with the commands:
```sh 
su npladm
startsap ALL
```
Then try and access your sap system using the GUI (which is included in the SAP rar downloaded files and just needs to be installed normally).

## Additonal Steps after successful installation

10. Once in the SAP GUI, Assign/ update the license for your SAP system.

Test the HTTP and HTTPS connection with your browser
HTTP: http://localhost:8000/sap/public/ping
HTTPS: https://localhost:44300/sap/public/ping


## Credit to the following people for the steps and the docker file:
* Tobias Hofmann - https://github.com/tobiashofmann/sap-nw-abap-docker.
* Nabi Zamani - https://github.com/nzamani/sap-nw-abap-trial-docker
* Gregor Wolf - https://bitbucket.org/gregorwolf/dockernwabap750/src/25ca7d78266bef8ed41f1373801fd5e63e0b9552/Dockerfile?at=master&fileviewer=file-view-default