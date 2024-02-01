#jenkins #installation


Update the system first
`sudo apt update`

Instructions are available on this page
https://pkg.jenkins.io/debian-stable/



# Jenkins Debian Packages

This is the Debian package repository of Jenkins to automate installation and upgrade. To use this repository, first add the key to your system (for the Weekly Release Line):

`sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \     [https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key](https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key)`
  

Then add a Jenkins apt repository entry:

`echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \     https://pkg.jenkins.io/debian-stable binary/ | sudo tee \     /etc/apt/sources.list.d/jenkins.list > /dev/null`
  

Update your local package index, then finally install Jenkins:

````
sudo apt-get update
````

````
sudo apt-get install fontconfig openjdk-17-jre
````

````
sudo apt-get install jenkins
````

Check if jenkins is running, if it's not running then run below command
`sudo systemctl status jenkins`

 enable the jenkins service
`sudo systemctl enable jenkins`



### Note: Accessing Jenkins dashboard if it is running through ec2 instance 

![[Pasted image 20231221214855.png]]

Add the above inbound rules to access successfully by default port 22 which is for ssh is enabled add the remaining type like HTTP, HTTPS also custom tcp 8080 too.

After the above changes dashboard is accessible at port 8080, for ec2 type
public_ip4_address:8080

![[Pasted image 20231221215325.png]]
The above dashboard is visible, now to access admin password go to terminal and type
`sudo cat /var/lib/jenkins/secrets/initialAdminPassword`

Install suggested plugins, create admin user and now its ready.


