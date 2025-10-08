# Java Application Build and Deployment process

we need two servers for this process:

*first server, for java application build purpose*

*second server, for Apache tomcat server installation plus deploy*

### Launch Java Build Server

1. go to aws console

2. create and download pem key

3. launch an ec2 instance with AMI - select Ubuntu server image

4. name it as javabuild-s

5. ![](Screenshot%202025-10-08%20092014.png)

   ------

   

###### select ubuntu image and make sure it is latest version i.e 24.04 LTS (HVM)

![](Screenshot%202025-10-08%20092029.png)

-----

###### create new key pair

![](Screenshot%202025-10-08%20092042.png)

------

###### save the javabuild-s.pem in you are local repository 

![](Screenshot%202025-10-08%20092113.png)

------

# AWS DEPLOY SERVER CREATION AND CONFIGURATION

#### LAUNCH EC2

> create an instance by giving instance name

![](Screenshot%202025-10-08%20092230.png)

------

###### select ubuntu image and make sure it is latest version i.e 24.04 LTS (HVM)

![](Screenshot%202025-10-08%20092246.png)

----

###### select the javabuild-s.pem

![](Screenshot%202025-10-08%20092307.png)

-----

> Use `cd downloads` to get into PEM file location which is saved in downloads
>
> Now paste the SSH key and enter yes you will be connected to DEPLOY server

![](Screenshot%202025-10-08%20092403.png)

----

###### Update the server using `sudo apt -y update`

![](Screenshot%202025-10-08%20092424.png)

-----

###### sever is updated 

![](Screenshot%202025-10-08%20092446.png)

---

> Open the GITHUB repo where JAVA code is stored in browser
>
> > Now copy the HTTPS URL(click on code down arrow to find the URL as shown in below image)

![](Screenshot%202025-10-08%20092535.png)

-----

###### Now in terminal: run `git clone <YOUR URL>` and your code is cloned(downloaded) in your BUILD server

###### use the following commands:

###### `ls` - to view the list of folders in repo

###### `cd <folder name>` - to change directory

![](Screenshot%202025-10-08%20092550.png)

---

###### Now update the system and install JAVA by using

> ```
> sudo apt install openjdk-21-jre-headless -y
> ```

![](Screenshot%202025-10-08%20092649.png)

---

###### now java openjdk-21-jre-headless installed

![](Screenshot%202025-10-08%20092720.png)

-----

> ###### Again use `ls` to view the files/folders present in the code repo read the POM file by using `cat pom.xml` and we get to know build tool and JAVA version developer mentioned

![](Screenshot%202025-10-08%20092755.png)

---

###### After Installing JAVA now install maven using

sudo  apt install maven


![](Screenshot%202025-10-08%20092825.png)

-----

> To build the code use this command:
>
> ```
> mvn package
> Build Success and code Build is completed
> ```

![](Screenshot%202025-10-08%20093039.png)

---

> To view the Artifact (.war) use `ls` and `cd target` ,`ls` again u can see .war file in red

![](Screenshot%202025-10-08%20093101.png)

------

> Use `cd downloads` to get into PEM file location which is saved in downloads
>
> Now paste the SSH key and enter yes you will be connected to DEPLOY server

![](Screenshot%202025-10-08%20093306.png)

----

# BUILD CODE DEPLOYMENT-TOMCAT WEB SERVER



> Update the server using `sudo apt -y update`

![](Screenshot%202025-10-08%20093350.png)

---

server is updated

![](Screenshot%202025-10-08%20093413.png)

------

Browse apache tomcat install and open the website

###### copy the address(URL Link) of tar.gz(pgp,sha512) file

![](Screenshot%202025-10-08%20093524.png)

------

###### Use `wget <tar.gz link address>` to download the folder

![](Screenshot%202025-10-08%20093633.png)

---

> By using `ls` u can find downloaded folder

![](Screenshot%202025-10-08%20093700.png)

---

to extract the folder use `tar -xvf apache-tomcat-9.0.110.tar.gz`

![](Screenshot%202025-10-08%20093827.png)

----

> Use `ls` to see the extracted folder
>
> Change directory to extracted folder `cd <folder name>`
>
> To see the files/folders inside extracted folder use `ls`

![](Screenshot%202025-10-08%20093848.png)

----

> Now change directory to bin folder `cd bin`
>
> Start TOMCAT server using `./startup.sh`

![](Screenshot%202025-10-08%20093912.png)

-----

> ###### Install JAVA latest version by using `sudo apt install openjdk-21-jre-headless` 

![](Screenshot%202025-10-08%20094009.png)

-----

> Now change directory to bin folder `cd bin`
>
> Start TOMCAT server using `./startup.sh`

![](Screenshot%202025-10-08%20094331.png)

---

First add a user for WEB server to do that

`ls` lists all the files/folders

change directory to `cd conf/` and `ls` to see all the files/folders present inside it

###### open file using `vi <file name>` here in the below image it is `vi tomcat-users.xml`

![](Screenshot%202025-10-08%20094402.png)

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

![](Screenshot%202025-10-08%20094529.png)

> configure the webapps
>
> change directory to `cd webapps/manager/META-INF` and `ls` to list the file to be configured
>
> open editor `vi <file name>` here in below image it is `vi context.xml`

![](Screenshot%202025-10-08%20094631.png)

----

> here remove the valve file

![](Screenshot%202025-10-08%20094722.png)

-----

Add port number 8080

Type: custom TCP ; Port range: 8080 ; Source: Anywhere IPV4

![](Screenshot%202025-10-08%20094833.png)

---

> Your TOMCAT Web server is started and you can see in the below image

![](Screenshot%202025-10-08%20095520.png)

-------

> open the browser and refresh the browser it will ask username and password
>
> Enter the valid username and password while u have given in adding users
>
> signin

![](Screenshot%202025-10-08%20095635.png)

------

> Now after configuration you can access manager in web server
>
> here you will see all the files present in webapps
>
> so our build code file is in build server , now you have to copy that file to DEPLOY server(webserver)

![](Screenshot%202025-10-08%20095758.png)

----

To copy from one server to another server you need to use SCP
```

scp -i pem.pem ubuntu@<source IP>:path ubuntu@<destination IP>:path

```
After this the BUILD CODE file in BUILD server is copied into webapps of the DEPLOY server

![](Screenshot%202025-10-08%20113315.png)

###### and file is copy from build server to deploy server

![](Screenshot%202025-10-08%20113339.png)

The Build code is successfully deployed

![](Screenshot%202025-10-08%20121816.png)
```







