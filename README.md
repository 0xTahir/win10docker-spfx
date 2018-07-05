# SPFx Docker image on Windows 10 VM

## Background
[Waldek Mastykarz](https://github.com/waldekmastykarz) did an amazing job of creating a [Docker image](https://github.com/waldekmastykarz/docker-spfx) for SharePoint Framework developement which helps in saving a lot of time for developers in terms of preparing the dev environment and running/loading up that environment everyday. 

He explained this on a Mac and I thought to give it a shot on a **Windows 10 (Enterprise) VM** inside **Azure** as most of the enterprise customers would be following this approach. This path led me to few challenges and this wiki talks about those challenges and their resolution.


This article assumes you have a basic knowledge of Azure VMs, dockers and SharePoint Framework Developement. Also go and check Waldek's [blog](https://blog.mastykarz.nl/sharepoint-framework-docker/) and [SharePoint PnP Webcast](https://dev.office.com/blogs/using-docker-in-sharepoint-framework-development) for a demo.

## Lets do it
1. Create a Windows 10 VM inside Azure. I have used [Azure Lab Services](https://docs.microsoft.com/en-us/azure/lab-services/) (formally known as DevTest Labs) using **Visual Studio Community 2017 on Windows 10 Enterprise N (x64)** as base image.

**Note:** Make sure you choose a **4 Core CPU** (e.g. D4S_V3 which is 4 Core & 16 GB in size) other wise Linux container wont run inside the Windows 10 VM and you will get error: _Failed to start the virtual machine 'MobyLinuxVM' because one of the Hyper-V components is not running_. [See details here](https://github.com/docker/for-win/issues/574).

![](https://github.com/AlgoNinja/win10docker-spfx/blob/master/01.PNG)

2. Log on to the newly created Azure Win10 VM and download and install [Docker for Windows](https://www.docker.com/docker-windows). You might be asked to logout and log back in to complete the installation.

3. After installation, Docker will ask you to enable Hyper-V and Containers features in the Win10 VM. Hit **Ok** and it will restart the VM too.

<img src="https://github.com/AlgoNinja/win10docker-spfx/blob/master/02.PNG" width="75%" height="75%">

4. Shut down the **Docker** service from the **System Tray** and launch it again from the desktop with **administrator** privileges.

<img src="https://github.com/AlgoNinja/win10docker-spfx/blob/master/03.PNG" width="75%" height="75%">
<img src="https://github.com/AlgoNinja/win10docker-spfx/blob/master/04.PNG" width="50%" height="50%">

5. Once Docker service is running, launch Windows **PowerShell** with **Administrative** privileges. Goto the project folder and run following command to download Waldek's image and run as Container.

`cd .\projects\spfx-helloworld\`

`docker run -it --rm --name spfx-helloworld -v /c/projects/spfx-helloworld:/usr/app/spfx -p 5432:5432 -p 4321:4321 -p 35729:35729 waldekm/spfx`

**Note:** You will be asked to share the host drive. Please select **Share it**. And then also provide the Windows 10 VM's password.

<img src="https://github.com/AlgoNinja/win10docker-spfx/blob/master/05.PNG" width="50%" height="50%">
<img src="https://github.com/AlgoNinja/win10docker-spfx/blob/master/06.PNG" width="75%" height="75%">

6. You will see the SPFx image is being downloaded from the github. This might take few minutes for the first time. Once the image is done downloading it will ran inside a container and you will be given the command prompt of the linux VM(which has all the SPFx dev tools pre-installed). 

![](https://github.com/AlgoNinja/win10docker-spfx/blob/master/07.PNG)

**Note:** Thost drives's **c:\Projects\spfx-helloworld** folder is mapped to **/user/app/spfx** folder in the container to persist the files beyond container's life.

7. You can create a new project by the running `yo @microsoft/sharepoint` command and all the files will be stored at c:\Projects\spfx-webpart which can be accessed on the Host VM(Win10 VM) via Visual Studio Code to [start the developement](https://docs.microsoft.com/en-us/sharepoint/dev/spfx/web-parts/get-started/build-a-hello-world-web-part).

![](https://github.com/AlgoNinja/win10docker-spfx/blob/master/08.PNG)

**Note:** You are ready to do developement in the Win10(Host VM) and Run the code inside the container(docker-spfx).

8. Once the project is ready run the `gulp serve` command in the container and then access the SharePoint workbench to test the web part from the Win10 VM at **https://localhost:5432/workbench**.

![](https://github.com/AlgoNinja/win10docker-spfx/blob/master/09.PNG)

Thats it. Have fun :o).
