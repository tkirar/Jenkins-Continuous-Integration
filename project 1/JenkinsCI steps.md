## Steps
userdata folder has the installation scripts for setting up these servers.
- Jenkins setup (ubuntu 20.04) t2.small [Runs on 8080 port]
- Nexus setup  (centos 7) t2.medium [Runs on 8081 port]
- Sonarqube Setup (ubuntu 20.04) t2.medium  [Directly runs on instance public IP]


  Kindly update :
  
- Security Setup
- Plugins
- Integrate
  -- Nexus
  --Sonarqube 

## Plugins for CI
- Nexus
- Sonarqube
- Git
- Pipeline Maven Integration Plugin
- build timestamp
- pipeline utility steps


## Step by step workflow:
1. Set up jenkins, nexus and sonarqube servers, with the help of installation scripts available in the userdata folder.
2. check and ensure that each service is running.
3. Install mentioned PLUGINS on jenkins UI --> manage jenkins-->plugins.
4. Also add tools jdk and sonarqube server. Also create authentication token on sonarqube for jenkins.

   Run the first Pipeline as a code script to perform code analysis.

   ![image](https://github.com/tkirar/Jenkins-Continuous-Integration/assets/69767391/0258c1a0-941c-405b-b9ec-047874937448)

   ![image](https://github.com/tkirar/Jenkins-Continuous-Integration/assets/69767391/55d011c2-6bc4-48b8-a6ca-9d755e12c800)


5. Now integrating the Nexus repository server to upload artifacts through Pipeline as a code script.
6. 

   
   

