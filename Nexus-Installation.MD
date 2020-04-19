# Nexus Installation
Nexus is one a artifact repository which helps to store your build outcomes.  

Follow this article in **[Youtube]()

Prerequisites
1. Make Sure You Have Java Installed (Refer To Java Requirement On Official Website)

# Implementation steps 
Download and setup nexus stable version

```sh 
cd /opt
wget https://download.sonatype.com/nexus/3/latest-unix.tar.gz
tar -zxvf  latest-unix.tar.gz
mv /opt/latest-unix.tar.gz /opt/nexus
```

As a good security practice, it is not advised to run nexus service as root. so create new user called nexus and grant sudo access to manage nexus services 

```sh 
sudo adduser nexus
# visudo \\ nexus   ALL=(ALL)       NOPASSWD: ALL
sudo chown -R nexus:nexus /opt/nexus
```

Open /opt/nexus/bin/nexus.rc file, uncomment run_as_user parameter and set it as following.

```sh 
vi /opt/nexus/bin/nexus.rc
run_as_user="nexus" (file shold have only this line)
```

Add nexus as a service at boot time
```sh
sudo ln -s /opt/nexus/bin/nexus /etc/init.d/nexus
```
Login as a nexus user and start service

```sh
su - nexus
service nexus start
```

Login nexus server from browser on port 8081
```sh
http://<Nexus_server>:8081
```
The path to default password will be provided such as "/opt/sonatype-work/nexus3/admin.password"

```sh
username : admin  
password : "/opt/sonatype-work/nexus3/admin.password"
```
#Troubleshooting

#service is not starting?
- make sure you are trying to start service with nexus user. 
- check java installation

Unable to access nexus URL?
- make sure port 8081 is opened in security group. 

Add the following For Troubleshooting 

Update File With Below Entry "/etc/security/limits.conf"

```sh
root soft memlock unlimited
root hard memlock unlimited
root - nofile 65536
root - nproc 4096
sonar - nofile 65536
sonar - nproc 4096
```

Update file with below entry  "/etc/sysctl.conf"

```sh
fs.file-max = 2097152
vm.max_map_count = 262144
vm.swappiness = 1
```

