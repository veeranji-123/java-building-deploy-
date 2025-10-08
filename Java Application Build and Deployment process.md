# Java Application Build and Deployment process

we need two servers for this process:

*first server, for java application build purpose*

*second server, for Apache tomcat server installation plus deploy*

### Launch Java Build Server

1. go to aws console

2. create and download pem key

3. launch an ec2 instance with AMI - select Ubuntu server image

4. name it as  javabuild-s![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 092014.png)

   ------

   

###### select ubuntu image and make sure it is latest version i.e 24.04 LTS (HVM)

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 092029.png)

-----

###### create new key pair

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 092042.png)

------

###### save the javabuild-s.pem in you are local repository 

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 092113.png)

------

# AWS DEPLOY SERVER CREATION AND CONFIGURATION

#### LAUNCH EC2

> create an instance by giving instance name

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 092230.png)

------

###### select ubuntu image and make sure it is latest version i.e 24.04 LTS (HVM)

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 092246.png)

----

###### select the javabuild-s.pem

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 092307.png)

-----

> Use `cd downloads` to get into PEM file location which is saved in downloads
>
> Now paste the SSH key and enter yes you will be connected to DEPLOY server

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 092403.png)

----

###### Update the server using `sudo apt -y update`

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 092424.png)

-----

###### sever is updated 

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 092446.png)

---

> Open the GITHUB repo where JAVA code is stored in browser
>
> > Now copy the HTTPS URL(click on code down arrow to find the URL as shown in below image)

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 092535.png)

-----

###### Now in terminal: run `git clone <YOUR URL>` and your code is cloned(downloaded) in your BUILD server

###### use the following commands:

###### `ls` - to view the list of folders in repo

###### `cd <folder name>` - to change directory

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 092550.png)

---

###### Now update the system and install JAVA by using

> ```
> sudo apt install openjdk-21-jre-headless -y
> ```

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 092649.png)

---

###### now java openjdk-21-jre-headless installed

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 092720.png)

-----

> ###### Again use `ls` to view the files/folders present in the code repo read the POM file by using `cat pom.xml` and we get to know build tool and JAVA version developer mentioned

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 092755.png)

---

###### After Installing JAVA now install maven using

```
sudo  apt install maven 
```

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 092825.png)

-----

> To build the code use this command:
>
> ```
> mvn package
> Build Success and code Build is completed
> ```

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 093039.png)

---

> To view the Artifact (.war) use `ls` and `cd target` ,`ls` again u can see .war file in red

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 093101.png)

------

> Use `cd downloads` to get into PEM file location which is saved in downloads
>
> Now paste the SSH key and enter yes you will be connected to DEPLOY server

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 093306.png)

----

# BUILD CODE DEPLOYMENT-TOMCAT WEB SERVER



> Update the server using `sudo apt -y update`

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 093350.png)

---

server is updated

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 093413.png)

------

Browse apache tomcat install and open the website

###### copy the address(URL Link) of tar.gz(pgp,sha512) file

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 093524.png)

------

###### Use `wget <tar.gz link address>` to download the folder

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 093633.png)

---

> By using `ls` u can find downloaded folder

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 093700.png)

---

to extract the folder use `tar -xvf apache-tomcat-9.0.110.tar.gz`

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 093827.png)

----

> Use `ls` to see the extracted folder
>
> Change directory to extracted folder `cd <folder name>`
>
> To see the files/folders inside extracted folder use `ls`

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 093848.png)

----

> Now change directory to bin folder `cd bin`
>
> Start TOMCAT server using `./startup.sh`

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 093912.png)

-----

> ###### Install JAVA latest version by using `sudo apt install openjdk-21-jre-headless 

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 094009.png)

-----

> Now change directory to bin folder `cd bin`
>
> Start TOMCAT server using `./startup.sh`

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 094331.png)

---

First add a user for WEB server to do that

`ls` lists all the files/folders

change directory to `cd conf/` and `ls` to see all the files/folders present inside it

###### open file using `vi <file name>` here in the below image it is `vi tomcat-users.xml`

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 094402.png)

---

> Add User at bottom above
>
> ```
> <role rolename="manager-gui"/>
> <role rolename="manager-script"/>
> <user username="admin" password="admin123" roles="manager-gui,manager-script"/>
> ```
>
> username and password can be anything depending upon user and save it using `:wq`

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 094529.png)

> configure the webapps
>
> change directory to `cd webapps/manager/META-INF` and `ls` to list the file to be configured
>
> open editor `vi <file name>` here in below image it is `vi context.xml`

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 094631.png)

----

> here remove the valve file

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 094722.png)

-----

Add port number 8080

Type: custom TCP ; Port range: 8080 ; Source: Anywhere IPV4

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 094833.png)

---

> Your TOMCAT Web server is started and you can see in the below image

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 095520.png)

-------

> open the browser and refresh the browser it will ask username and password
>
> Enter the valid username and password while u have given in adding users
>
> signin

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 095635.png)

------

> Now after configuration you can access manager in web server
>
> here you will see all the files present in webapps
>
> so our build code file is in build server , now you have to copy that file to DEPLOY server(webserver)

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 095758.png)

----

To copy from one server to another server you need to use SCP

```
scp -i pem.pem ubuntu@<source IP>:path ubuntu@<destination IP>:path
```

After this the BUILD CODE file in BUILD server is copied into webapps of the DEPLOY server

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 113315.png)

###### and file is copy from build server to deploy server

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 113339.png)

The Build code is successfully deployed

![](C:\Users\veera\OneDrive\Pictures\Screenshots\Screenshot 2025-10-08 121816.png)







