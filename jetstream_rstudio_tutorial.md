<!---

Images can be added in-line as a reStructured text substitution, but will not render in Markdown. See reStructured text example. http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html#substitution-definitions
--->
|CyVerse logo|

# Setting up a RStudio-Server on an Atmosphere/Jetstream instance

##### Content Contributors: Tyson Lee Swetnam
##### Lesson Maintainers: Tyson Lee Swetnam <tswetnam@cyverse.org>
##### Lesson status: under development
## Goal

This tutorial is for installing and launching [RStudio-Server](https://www.rstudio.com/products/rstudio/) from Atmosphere / Jetstream cloud instances.

Once it is running RStudio-Server is accessed via a web browser pointed to the instance IP address and a port number (default for Rstudio-Server is `<IP-ADDRESS>:8787`).

For those interested in working in an existing image with R and RStudio-Server installed, check out the quick start [Launching RStudio in Atmosphere/Jetstream](newhtmlhere).

For those interested in doing analyses with R and RStudio, check out these amazing R tutorials in the [Data Carpentry Worshop Series](http://www.datacarpentry.org/lessons/).  

## Prerequisites
<!---
Insert short description
--->


*You may find the following tutorials useful for completing this one:*

|Related tutorial|Description|Source|
|----------------|-----------|----|
|Create CyVerse Account Quickstart||[CyVerse](https://user.cyverse.org/)
|Create XSEDE Account Quickstart||[XSEDE](https://www.xsede.org/)
|Installing CyberDuck||[CyberDuck](https://cyberduck.io/)
|Setting up iRODS iCommands||[iCommands](https://pods.iplantcollaborative.org/wiki/display/DS/Using+iCommands)

<!---
**Example data citation:**

Links to papers, SRA, etc. 
--->

### Platform(s)

<!---
Keep only the relevant entries and delete/hide the remaining
--->

*We will use the following CyVerse platform(s):*

|Platform|Interface|Link|Platform Documentation|
|--------|---------|----|----------------------|
|Discovery Environment|Web/Point-and-click|[Discovery Environment](https://de.iplantcollaborative.org)|[Manual](https://pods.iplantcollaborative.org/wiki/display/DEmanual/Table+of+Contents)|
|Atmosphere|Command-line (ssh) and/or Desktop (VNC)|[Atmosphere](https://atmo.cyverse.org)|[Manual](https://pods.iplantcollaborative.org/wiki/display/atmman/Atmosphere+Manual+Table+of+Contents)|
|Jetstream|Command-line (ssh) and/or Desktop (VNC)|[Jetstream](https://use.jetstream-cloud.org/)|[Manual](https://iujetstream.atlassian.net/wiki/display/JWT/How-to+articles), [Wiki](http://wiki.jetstream-cloud.org/)|



---

## Overview

<!---
Text and workflow image go here. Using reStructured text, we can place a link to an image in pipe form (label images 'image n' with n=0 being image 0 and so on). At the end of the document add the image names, links, and parameters. 
--->

<!---

Images can be added in-line as a reStructured text substitution, but will not render in Markdown. See reStructured text example here. http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html#substitution-definitions

|image 0|

--->
The first step is to launch an instance with an OS - RStudio-Server can run on either Linux Redhat/CentOS or Debian/Ubuntu.

### Apps and Atmosphere images

**Discovery Environment App(s):**

<!---
Links to Apps in the DE which are found by clicking the INFO button; select and copy App URL
--->

|App name|Version|Description|App Link|
|--------|-------|-----------|--------|
|Data Store|current|Click 'Data' tab|[Managing Data in Cyverse](http://www.cyverse.org/learning-center/manage-data)


**Tested Atmosphere Image(s):**

|Image name|Version|Description|Link|
|----------|-------|-----------|----|
|CentOS 6.8|
|CentOS 7.2||
|Ubuntu 14.04.2 XFCE R QGIS|1.0|Ubuntu 14.04.3 -Trusty Tahr|[Image](https://atmo.cyverse.org/application/images/1335)|
|Ubuntu 16.04 GUI XFCE Base|1.0|Ubuntu 16.04 - Xenial Xerus|[Image](https://atmo.cyverse.org/application/images/1453)|

**Jetstream Image(s):**

|Image name|Version|Description|Link|
|----------|-------|-----------|----|
|CentOS 6.8|1.8||[Image](https://use.jetstream-cloud.org/application/images/61)|
|CentOS 7.2|1.2|*Requires a m1.small or larger VM*|[Image](https://use.jetstream-cloud.org/application/images/161)|
|Ubuntu 14.04.03 Development|1.1|Ubuntu 14.04.3 -Trusty Tahr|[Image](https://use.jetstream-cloud.org/application/images/99)|
|Ubuntu 16.02||

## Directions

<!---

--->

### Launch an Atmosphere or Jetstream instance

**Task:** Log into [Atmosphere](https://atmo.cyverse.org) or [Jetstream](https://use.jetstream-cloud.org) and select a CentOS or Ubuntu image. This tutorial has only been tested with the listed images (*see above*). Select the appropriate instance size (recommend medium or larger). Launch the instance. 

Once the instance is available you can SSH into the instance using Terminal.

You should reset the password for the `user_name` and `root`:

```
sudo passwd <user_name here>
```
To add a `new_user_name`:

```
sudo adduser <new_user_name>
```
Logging in as the ```root``` super user you can give the new user ```sudo``` priveleges

``` 
su 
```

On CentOS:
```
usermod -aG wheel <new_user_name>
```

On Ubuntu:
```
usermod -aG sudo <new_user_name> 
```

---

# Installing R and RStudio-Server

## Version Checks

*In order to complete this tutorial you will want the most up to date .rpm or .deb files for R and RStudio-Server:*

|Prerequisite|Preparation/Notes|Link|
|------------|-----------------|-------------|
|R|Check for the latest version|[CRAN R-Project](https://cran.r-project.org/)|
|Microsoft R Open|Check MRAN for the latest version|[MRAN R](https://mran.microsoft.com/documents/rro/installation/#revorinst-lin)
|RStudio-Server|Check the for latest version|[RStudio-Server](https://www.rstudio.com/products/rstudio/download-server/)|

##CentOS
**Task:** Installing R & RStudio-Server in CentOS Example (*check for updated versions in table above and update code below as needed*) 

```
sudo yum -y update

sudo yum-builddep R

sudo yum -y install R
```
```
wget https://download2.rstudio.org/rstudio-server-rhel-1.0.136-x86_64.rpm
sudo yum -y install --nogpgcheck rstudio-server-rhel-1.0.136-x86_64.rpm
sudo rm *.rpm
```
**Task:** If iRODS are already installed on the VM skip this step. If not, install [iRODS](http://irods.org/download/)
 (*check for updated versions in table above and update code below as needed*) 

```
sudo rpm -i irods-icat-4.1.10-centos7-x86_64.rpm irods-database-plugin-postgres93-1.10-centos7-x86_64.rpm
```

Upgrade:

```
sudo rpm -U irods-database-plugin-postgres93-1.10-centos7-x86_64.rpm
sudo rpm -U irods-icat-4.1.10-centos7-x86_64.rpm
```

Initialize iRODS

```
iinit
```
Enter your CyVerse information 
*Note: As of 1/2017 the CyVerse Datastore still uses an iPlantCollaborative portal.*

```
Enter the host name (DNS) of the server to connect to: data.iplantcollaborative.org
Enter the port number: 1247
Enter your irods user name: user_name_here
Enter your irods zone: iplant
Those values will be added to your environment file (for use by
other iCommands) if the login succeeds.

Enter your current iRODS password:
```

##Ubuntu
**Task:** Add the r-base keyserver

```
sudo sh -c 'echo "deb http://cran.rstudio.com/bin/linux/ubuntu trusty/" >> /etc/apt/sources.list' &&
gpg --keyserver keyserver.ubuntu.com --recv-key E084DAB9 &&
gpg -a --export E084DAB9 | sudo apt-key add - &&
sudo apt-get update &&
sudo apt-get -y install r-base r-base-dev
```
**Task:** Install RStudio-Server in Ubuntu (*check for updated version above and update code below*) 

```
sudo apt-get -y install libapparmor1 gdebi-core &&
wget https://download2.rstudio.org/rstudio-server-1.0.143-amd64.deb
```
```
sudo gdebi -y rstudio-server-1.0.143-amd64.deb &&
sudo dpkg -i *.deb && 
rm *.deb
```

**Task:** Install iCommands

```
wget ftp://ftp.renci.org/pub/irods/releases/4.1.10/ubuntu14/irods-icommands-4.1.10-ubuntu14-x86_64.deb &&
sudo gdebi irods-icommands-4.1.10-ubuntu14-x86_64.deb &&
sudo dpkg -i *.deb && 
rm *.deb
```

## Starting RStudio-Server and checking status

[Configuring RStudio-Server](https://support.rstudio.com/hc/en-us/articles/200552316-Configuring-the-Server)

RStudio should now be running on your VM. In the SSH Terminal you can query to see if RStudio-Server is running

```
rstudio-server status
```

If RStudio is not running you can start it:

```
rstudio-server start
```

Next, Open your web browser.
Type the IP Address of the VM running RStudio into your browser's console and use the default port # 8787.

RStudio should ask for your username and password. These should be the same as your default username and password unless you edited them using the directions given above.

```
<IP Address here>:8787
```

## Summary

In this tutorial we show how to install RStudio-Server on a Linux VM

<!---
Summary and example figures
--->

**Next Steps:**



## FAQ


<!---
Optional list of one or more FAQ questions
--->

1. **Question:** How do I move my data onto or off the VM?

	a. Use the wget or curl commands within RStudio to move your data. 

	b. Use CyberDuck to SSH to the VM using the <IP ADDRESS>:<PORT 22>

	c. Use the iRODS iCommands to move your data from the CyVerse Data Store onto your VM.
    	
2. **Question:**
    a. 
    

## Setting up RStudio-Server with the `ezj -R` function on Atmosphere

Recently we set up a `ez` script which is executed in the terminal or web shell to install Anaconda3 with Jupyter Notebook. One of the features is to install R kernel with Jupyter. It also installs `r-essentials` with numerous common R packages.

To set up RStudio-Server with the `conda` installation of R you need to set up the bash profile.

1. Add `anaconda3` to your path

```
$ export PATH="/home/anaconda3/bin:$PATH"
```

2. Add to your `~/.bash_profile`:

```
$ sudo sh -c 'echo "export RSTUDIO_WHICH_R="/home/anaconda3/bin/R"" >> ~/.bash_profile' &&
```

3. Install RStudio-Server using the [latest version](https://www.rstudio.com/products/rstudio/download-server/)

```
$ sudo apt-get install gdebi-core
$ cd /home
$ wget https://download2.rstudio.org/rstudio-server-1.0.143-amd64.deb
$ sudo gdebi -n rstudio-server-1.0.143-amd64.deb
```

Note - this will fail on the first try. 
```
user_name@128:/home$ sudo gdebi rstudio-server-1.0.143-amd64.deb
Reading package lists... Done
Building dependency tree
Reading state information... Done
Reading state information... Done

RStudio Server
 RStudio is a set of integrated tools designed to help you be more productive with R. It includes a console, syntax-highlighting editor that supports direct code execution, as well as tools for plotting, history, and workspace management.
Do you want to install the software package? [y/N]:y
(Reading database ... 136874 files and directories currently installed.)
Preparing to unpack rstudio-server-1.0.143-amd64.deb ...
Unpacking rstudio-server (1.0.143) over (1.0.143) ...
Setting up rstudio-server (1.0.143) ...
useradd: user 'rstudio-server' already exists
groupadd: group 'rstudio-server' already exists
rsession: no process found
Created symlink from /etc/systemd/system/multi-user.target.wants/rstudio-server.service to /etc/systemd/system/rstudio-server.service.
Job for rstudio-server.service failed because the control process exited with error code. See "systemctl status rstudio-server.service" and "journalctl -xe" for details.
● rstudio-server.service - RStudio Server
   Loaded: loaded (/etc/systemd/system/rstudio-server.service; enabled; vendor preset: enabled)
   Active: active (running) since Sat 2017-05-13 09:30:40 MST; 13ms ago
  Process: 2226 ExecStop=/usr/bin/killall -TERM rserver (code=exited, status=1/FAILURE)
  Process: 2233 ExecStart=/usr/lib/rstudio-server/bin/rserver (code=exited, status=0/SUCCESS)
 Main PID: 2236 (rserver)
    Tasks: 3
   Memory: 824.0K
      CPU: 10ms
   CGroup: /system.slice/rstudio-server.service
           └─2236 /usr/lib/rstudio-server/bin/rserver

May 13 09:30:40 128.196.64.129 systemd[1]: rstudio-server.service: Service hold-off time over, scheduling restart.
May 13 09:30:40 128.196.64.129 systemd[1]: Stopped RStudio Server.
May 13 09:30:40 128.196.64.129 systemd[1]: Starting RStudio Server...
May 13 09:30:40 128.196.64.129 systemd[1]: Started RStudio Server.
May 13 09:30:40 128.196.64.129 rserver[2236]: ERROR Unable to find an installation of R on the system (which R didn't return va...pp:472
May 13 09:30:40 128.196.64.129 systemd[1]: rstudio-server.service: Main process exited, code=exited, status=1/FAILURE
Hint: Some lines were ellipsized, use -l to show in full.
```


4. modify `/etc/rstudio/rserver.conf`

```
sudo sh -c 'echo "rsession-which-r=/home/anaconda3/bin/R" >> /etc/rstudio/rserver.conf'
```

5. Restart RStudio-Server

```
rstudio-server start
```

6. There were a couple of issues installing packages for the first time in RStudio

In R:
```
options(repos='http://cran.rstudio.com/')
options(download.file.method = "wget")
```

In terminal:
```
sudo ln -s /usr/lib/x86_64-linux-gnu/libgfortran.so.3 /usr/lib/libgfortran.so
```

## More help/additional information

<!---
Short description and links to any reading materials
--->

**Search for an answer:** [CyVerse Learning Center](http://www.cyverse.org/learning-center) or [Wiki](https://wiki.cyverse.org/wiki/dashboard.action)
**Post your question to the user forum:** [Ask CyVerse](http://ask.iplantcollaborative.org/questions/)


### Fix or improve this tutorial 


**Fix this tutorial on GitHub:** [GitHub](Link_to_gh_readme)
**Send a note to support:** [Tutorials@CyVerse.org](mailto:Tutorials@CyVerse.org)

<!---

SAMPLE DIRECTIVES (DELETE UNSUED ONES)
--------------------------------------

See: http://docutils.sourceforge.net/docs/ref/rst/directives.html#admonitions

.. Danger::
	This step is dangerous

.. Important::
	This step is important
	
.. Caution::
	Exercise caution
	
.. Hint::
	This is a hint

.. Important::
	This is very important

.. note:: This is a note admonition.
   This is the second line of the first paragraph.

   - The note contains all indented body elements
     following.
   - It includes this bullet list.



.. |CyVerse logo| image:: ./img/cyverse_rgb.png
    :width: 500
    :height: 100
--->
