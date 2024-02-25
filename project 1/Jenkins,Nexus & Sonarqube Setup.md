# Jenkins,Nexus & Sonarqube Setup
## Settiing up Jenkins Server
- ubuntu 20.04
- t2.small
- 10GB Disc space

## Commands executed to install jenkins on server

### Each commands can be run 0ne by one or As a whole by creating an executable .sh file

---
#!/bin/bash

sudo apt update

sudo apt install openjdk-11-jdk -y

sudo apt install maven wget unzip -y

curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
  
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt-get update

sudo apt-get install jenkins -y

---

## Setting Up Nexus Server
- CentOS7 or CentOS9 ami
- t2.medium
- create nexus Securitu group: nexusSG
- Add port 22 from my IP
- Add port 8081 from Anyhere or 8081 from jenkinsSG
- Add the following nexus script in user data.

---
#!/bin/bash

yum install java-1.8.0-openjdk.x86_64 wget -y   

mkdir -p /opt/nexus/   

mkdir -p /tmp/nexus/                           

cd /tmp/nexus/

NEXUSURL="https://download.sonatype.com/nexus/3/latest-unix.tar.gz"

wget $NEXUSURL -O nexus.tar.gz

sleep 10


EXTOUT=`tar xzvf nexus.tar.gz`

NEXUSDIR=`echo $EXTOUT | cut -d '/' -f1`

sleep 5

rm -rf /tmp/nexus/nexus.tar.gz

cp -r /tmp/nexus/* /opt/nexus/

sleep 5

useradd nexus

chown -R nexus.nexus /opt/nexus 

cat <<EOT>> /etc/systemd/system/nexus.service

[Unit]                                                                          
Description=nexus service                                                       
After=network.target                                                            
                                                                  
[Service]                                                                       
Type=forking                                                                    
LimitNOFILE=65536                                                               
ExecStart=/opt/nexus/$NEXUSDIR/bin/nexus start                                  
ExecStop=/opt/nexus/$NEXUSDIR/bin/nexus stop                                    
User=nexus                                                                      
Restart=on-abort                                                                
                                                                  
[Install]                                                                       
WantedBy=multi-user.target                                                      

EOT

echo 'run_as_user="nexus"' > /opt/nexus/$NEXUSDIR/bin/nexus.rc

systemctl daemon-reload

systemctl start nexus

systemctl enable nexus

---
  


