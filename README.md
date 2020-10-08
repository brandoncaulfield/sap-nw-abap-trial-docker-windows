# SAP NW ABAP Trial in Docker on Windows

This guide outlines how to install the SAP NW ABAB Developer Trial 7.51+ using Docker on a Windows 10 machine.

#### [YouTube Tutorial](https://youtu.be/fL4XkwB617w). 


### Credit to the following people for the steps and the docker file:

[Tobias Hofmann](https://github.com/tobiashofmann/sap-nw-abap-docker)

[Nabi Zamani](https://github.com/nzamani/sap-nw-abap-trial-docker)

[Gregor Wolf](https://bitbucket.org/gregorwolf/dockernwabap750/src/25ca7d78266bef8ed41f1373801fd5e63e0b9552/Dockerfile?at=master&fileviewer=file-view-default)


## Steps:

1. Clone this GitHub repo

```sh
git clone https://github.com/brandoncaulfield/sap-nw-abap-trial-docker-windows

```

2. Downlaod the [SAP NW ABAP Trial .rar files](https://developers.sap.com/trials-downloads.html)

3. Create a new folder in the local repo folder that you just cloned called **sapdownloads**

4. Copy the extracted rar files to the **sapdownloads** folder

5. Open Command Prompt or PowerShell and navigate to the local repo folder and run the docker build command:

```sh
docker build -t nwabap:7.52 .
```

6. Once the build has finished you need to adjust your **vm.max_map_count=1000000**. From a new Command Prompt or Powershell window run the following commands:

```sh
wsl -d docker-desktop
sysctl -w vm.max_map_count=1000000
```

Check that the vm.max_map_count has actually changed by running this command:

```sh
sysctl vm.max_map_count
```

7. Then run your docker container

```sh
docker run -p 8000:8000 -p 44300:44300 -p 3300:3300 -p 3200:3200 -h vhcalnplci --name nwabap752 -it nwabap:7.52 /bin/bash
```

8. Once your container is running you need to begin installing the SAP system. The password you select during the installation should be at least 8 characters long. This could take a while so be patient! :)

```sh
/usr/sbin/uuidd
./install.sh
```

9. Once the SAP system is installed successfully start the new SAP system by running the following commands:

```sh
su npladm
startsap ALL
```

Then try and access your sap system using the GUI which is included in the SAP rar downloaded files (just needs to be installed normally).


## Important Post Installation Steps
These steps were copied directly from Nabi Zamani's [GitHub Repo](https://github.com/nzamani/sap-nw-abap-trial-docker).
1.  Updating License
    -   Open SAP GUI and logon
        -   **User:**  SAP*
        -   **Password:**  Down1oad
        -   **Client:**  000
    -   Open Transaction  `SLICENSE`
    -   From the Screen copy the value of field  `Active Hardware Key`
    -   Go to  [SAP License Keys for Preview, Evaluation, and Developer Versions](https://go.support.sap.com/minisap/#/minisap)  in your browser
    -   Choose  `NPL - SAP NetWeaver 7.x (Sybase ASE)`
    -   Fill out the fields. Use the  `Hardware Key`  you copied from  `SLICENSE`
    -   Keep the downloaded file  `NPL.txt`  and go back to the  `SLICENSE`
    -   Delete the  `Installed License`  from the table
    -   Press the button  `Install`  below the table
    -   Choose the downloaded file  `NPL.txt`
    -   Done - happy learning. Now logon with the dev user.
        
    
    You can now logon to  `client 001`  with any of the following users (all share the same password  `Down1oad`, typically you would work with  `DEVELOPER`):
    
    -   **User:**  DEVELOPER (Developer User)
    -   **User:**  BWDEVELOPER (Developer User)
    -   **User:**  DDIC (Data Dictionary User)
    -   **User:**  SAP* (SAP Administrator)
2.  Generating Test Data
    
    Execute the following to generate some test data:
    
    -   **Report:**  SAPBC_DATA_GENERATOR
    -   **Transaction Code:**  SEPM_DG
3.  Suggestion: Activate the good old ping service
    
    -   Go to Transaction  `SICF`
    -   Activate the node  `/sap/public/ping`  (default_host)
    -   Test the HTTP and HTTPS connection with your browser
        -   **HTTP:**  [http://localhost:8000/sap/public/ping](http://localhost:8000/sap/public/ping)
        -   **HTTPS:**  [https://localhost:44300/sap/public/ping](https://localhost:44300/sap/public/ping)
