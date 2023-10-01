### Unable to Update CentOS7
OS: CentOS 7 or 8
Type: linux
Description: Cant Update system
Solution Source: https://stackoverflow.com/questions/70963985/error-failed-to-download-metadata-for-repo-appstream-cannot-prepare-internal

```
yum update -y
Error: Failed to download metadata for repo 'appstream': Cannot prepare internal mirrorlist: No URLs in mirrorlist
```

Solution: 
Go to `/etc/yum.repos.d/`

Run:

```
sudo sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*`
sudo sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*
`sudo yum update -y
```

