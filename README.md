# Jenkins, Ubuntu 16.04 (xenial), Vagrant
> **Note:** Some steps of this guide were taken from the Jenkins doc. So, if something is missing or if you want more information, you can visit the official documentation  [here](https://jenkins.io/doc/book/installing/#debian-ubuntu).

The whole process takes place on the vagrant virtual machine so we need to access to our vm:
```
$ vagrant ssh
```

Before installing jenkins we need a JRE and JDK installed. For this guide i've used openjdk-8 which is provided by Ubuntu. 
```
$ sudo apt-get install openjdk-8-jre openjdk-8-jdk
```
Jenkins can be installed through **apt**. So, we can add the repository as follows:
```
$ wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
$ sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
```
update
```
$ sudo apt-get update
```
and then install Jenkins
```
$ sudo apt-get install jenkins
```
This package installation will:
- Setup Jenkins as a daemon launched on start. See /etc/init.d/jenkins for more details.
- Create a jenkins user to run this service.
- Direct console log output to the file /var/log/jenkins/jenkins.log. Check this file if you are troubleshooting Jenkins.
- Populate /etc/default/jenkins with configuration parameters for the launch, e.g JENKINS_HOME
- Set Jenkins to listen on port 8080. Access this port with your browser to start configuration.

> If your /etc/init.d/jenkins file fails to start Jenkins, edit the /etc/default/jenkins to replace the line ----HTTP_PORT=8080---- with ----HTTP_PORT=8081---- Here, "8081" was chosen but you can put another port available.

In order to access locally through the browser, we need to add some lines to our **Vagrantfile**. So, open the file with your favorite text editor and look for this line:
```
# config.vm.network "forwarded_port", guest: 80, host: 8080
```
The **#** symbol tells us that the line is commented. So, we need to uncomment the line and change the **guest** value as follows:
```
config.vm.network "forwarded_port", guest: 8080, host: 8001
```
Why port 8080? The package installation set jenkins to listen on port 8080. If you want to set another port, it's up to you. For the host value you can also use a different port. I've chosen port 8001 just for a personal convention.

Once you have configured your vagrant file, reload your vagrant configuration.
```
$ vagrant reload
```
and then access again to the vm
```
$ vagrant ssh
```
Before start our first configuration we need to know the password Jenkins generated for to do it, so logged as root execute:
```
# cat /var/lib/jenkins/secrets/initialAdminPassword
```
this will print a password that we have to use once. Go to your browser and type: [http://localhost:8001/](http://localhost:8001/) and follow the firsrun instructions and That's all!
   
### References:
[https://jenkins.io/doc/book/installing/#debian-ubuntu](https://jenkins.io/doc/book/installing/#debian-ubuntu)
[https://wiki.jenkins.io/display/JENKINS/Installing+Jenkins+on+Ubuntu](https://wiki.jenkins.io/display/JENKINS/Installing+Jenkins+on+Ubuntu)
[https://www.digitalocean.com/community/tutorials/how-to-install-jenkins-on-ubuntu-16-04](https://www.digitalocean.com/community/tutorials/how-to-install-jenkins-on-ubuntu-16-04)
[https://www.jorgedelacruz.es/2017/02/14/jenkins-2-instalacion-de-jenkins-en-ubuntu/](https://www.jorgedelacruz.es/2017/02/14/jenkins-2-instalacion-de-jenkins-en-ubuntu/)