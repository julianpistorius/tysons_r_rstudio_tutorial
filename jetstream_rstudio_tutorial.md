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

Once it is running RStudio-Server is accessed via a web browser pointed to the instance IP address and port number.

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
|CentOS 6.8||
|CentOS 7.2||
|Ubuntu 14.04.2 XFCE R QGIS|1.0|Ubuntu 14.04.3 -Trusty Tahr|[Image](https://atmo.cyverse.org/application/images/1335)|
|Ubuntu 16.02||

**Jetstream Image(s):**

|Image name|Version|Description|Link|
|----------|-------|-----------|----|
|CentOS 6.8||
|CentOS 7.2|1.2|*Requires a m1.small or larger VM*|[Image](https://use.jetstream-cloud.org/application/images/161)|
|Ubuntu 14.04.03 Development|1.1|Ubuntu 14.04.3 -Trusty Tahr|[Image](https://use.jetstream-cloud.org/application/images/99)|
|Ubuntu 16.02||

## Directions

<!---

--->

### Launch an Atmosphere or Jetstream instance

**Task:** Log into [Atmosphere](https://atmo.cyverse.org) or [Jetstream](https://use.jetstream-cloud.org) and select a CentOS or Ubuntu image. This tutorial has only been tested with the listed images (*see above*). Select the appropriate instance size (recommend medium or larger). Launch the instance. 

Once the instance is available you can SSH into the instance using Terminal.

You need to reset the password on the VM, and add users as needed:

```
sudo passwd <username here>
```

```
sudo adduser <new username here>
```
Logging in as the ```root``` super user you can give the new user ```sudo``` priveleges

On CentOS:
```
usermod -aG wheel <username here>
```

On Ubuntu:
```
usermod -aG sudo <username here> 
```

---

# Installing R and RStudio-Server

## Version Checks

*In order to complete this tutorial you will want the most up to date .rpm or .deb files for R and RStudio-Server:*

|Prerequisite|Preparation/Notes|Link|
|------------|-----------------|-------------|
|R|Check for the latest version|[CRAN R-Project](https://cran.r-project.org/)|
|RStudio-Server|Check the for latest version|[RStudio-Server](https://www.rstudio.com/products/rstudio/download-server/)|
|iRODS|Check for the latest version|[iRODS](http://irods.org/download/)

##CentOS
**Task:** Installing R & RStudio-Server in CentOS Example (*check for updated versions in table above and update code below as needed*) 

```
sudo yum update
sudo yum install R
```
```
wget https://download2.rstudio.org/rstudio-server-rhel-1.0.44-x86_64.rpm
sudo yum install --nogpgcheck rstudio-server-rhel-1.0.44-x86_64.rpm
sudo rm *.rpm
```
**Task:** Install iRODS (*check for updated versions in table above and update code below as needed*) 

```
sudo rpm -i irods-icat-4.1.10-centos7-x86_64.rpm irods-database-plugin-postgres93-1.10-centos7-x86_64.rpm
```

Upgrade:

```
sudo rpm -U irods-database-plugin-postgres93-1.10-centos7-x86_64.rpm
sudo rpm -U irods-icat-4.1.10-centos7-x86_64.rpm
```

##Ubuntu
**Task:** Add the r-base keyserver

```
sudo sh -c 'echo "deb http://cran.rstudio.com/bin/linux/ubuntu trusty/" >> /etc/apt/sources.list' &&
gpg --keyserver keyserver.ubuntu.com --recv-key E084DAB9 &&
gpg -a --export E084DAB9 | sudo apt-key add - &&
sudo apt-get update &&
sudo apt-get -y install r-base
```
**Task:** Install RStudio-Server in Ubuntu (*check for updated version above and update code below*) 

```
sudo apt-get -y install libapparmor1 gdebi-core &&
wget https://download2.rstudio.org/rstudio-server-0.99.903-amd64.deb &&
sudo gdebi rstudio-server-0.99.903-amd64.deb &&
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

## Testing

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
