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

### [Communicate different pods using Podman](https://stackoverflow.com/questions/71141056/communicate-different-pods-using-podman)

I am deploying several Pods using podman-compose. To do so, each pod has its own definition in a podman-compose.yaml file that I execute in rootless mode (so all containers in a Pod coexist in the same host/IP). However, I would like to make able a container in a Pod to reach a service exposed by a container in another pod.

Ans:
```
podman network create $NETWORK_NAME

podman pod create --name $POD1 --network $NETWORK_NAME

podman pod create --name $POD2 --network $NETWORK_NAME

podman run -it --detach --pod $POD1 --name $CONTAINER1 --network $NETWORK_NAME image_name

podman run -it --detach --pod $POD2 --name $CONTAINER2 --network $NETWORK_NAME image_name

```
