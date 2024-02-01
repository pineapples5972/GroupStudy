#jenkins #installation


Update the system first
`sudo apt update`

Jenkins requires java to run so install openjdk distribution
`sudo apt install openjdk-11-jre`

```
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \  
/usr/share/keyrings/jenkins-keyring.asc > /dev/null
```
`
```
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \  
https://pkg.jenkins.io/debian-stable binary/ | sudo tee \  
/etc/apt/sources.list.d/jenkins.list > /dev/null
```

`sudo apt-get update`

`sudo apt-get install jenkins`

`sudo apt-get update -y`

`sudo apt-get install jenkins -y`

Installation finished now enable the jenkins service
`sudo systemctl enable jenkins`

Check if jenkins is running
`sudo systemctl status jenkins`

### Note: Accessing Jenkins dashboard if it is running through ec2 instance 

![[Pasted image 20231221214855.png]]

Add the above inbound rules to access successfully by default port 22 which is for ssh is enabled add the remaining type like HTTP, HTTPS also custom tcp 8080 too.

After the above changes dashboard is accessible at port 8080, for ec2 type
public_ip4_address:8080

![[Pasted image 20231221215325.png]]
The above dashboard is visible, now to access admin password go to terminal and type
`sudo cat /var/lib/jenkins/secrets/initialAdminPassword`

Install suggested plugins, create admin user and now its ready.


