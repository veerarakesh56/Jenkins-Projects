# Jenkins-Projects


## Installation on EC2 Instance




Install Jenkins, configure Docker as agent, set up cicd, deploy applications to k8s and much more.

## AWS EC2 Instance

- Go to AWS Console
- Instances(running)
- Launch instances

<img width="994" alt="215974891-196abfe9-ace0-407b-abd2-adcffe218e3f" src="https://github.com/veerarakesh56/Jenkins-Projects/assets/171412850/44274d05-7a6c-4c0c-bd47-3ee20f453818">

### Install Jenkins.

Pre-Requisites:
 - Java (JDK)

### Run the below commands to install Java and Jenkins

Install Java

```
sudo apt update
sudo apt install openjdk-11-jre
```

Verify Java is Installed

```
java -version
```

Now, you can proceed with installing Jenkins

```
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

**Note: ** By default, Jenkins will not be accessible to the external world due to the inbound traffic restriction by AWS. Open port 8080 in the inbound traffic rules as show below.

- EC2 > Instances > Click on <Instance-ID>
- In the bottom tabs -> Click on Security
- Security groups
- Add inbound traffic rules as shown in the image (you can just allow TCP 8080 as well, in my case, I allowed `All traffic`).

<img width="1187" alt="215975712-2fc569cb-9d76-49b4-9345-d8b62187aa22" src="https://github.com/veerarakesh56/Jenkins-Projects/assets/171412850/cfaecf37-e576-49b5-8ec3-c48aa6290ba3">


### Login to Jenkins using the below URL:

http://<ec2-instance-public-ip-address>:8080    [You can get the ec2-instance-public-ip-address from your AWS EC2 console page]

Note: If you are not interested in allowing `All Traffic` to your EC2 instance
      1. Delete the inbound traffic rule for your instance
      2. Edit the inbound traffic rule to only allow custom TCP port `8080`
  
After you login to Jenkins, 
      - Run the command to copy the Jenkins Admin Password - `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`
      - Enter the Administrator password
      
<img width="1291" alt="215959008-3ebca431-1f14-4d81-9f12-6bb232bfbee3" src="https://github.com/veerarakesh56/Jenkins-Projects/assets/171412850/1e831900-09a1-4894-9e1b-b70ad12ff012">

### Click on Install suggested plugins

<img width="1291" alt="215959294-047eadef-7e64-4795-bd3b-b1efb0375988" src="https://github.com/veerarakesh56/Jenkins-Projects/assets/171412850/264bec0f-3f66-4b33-9e38-d87eef02f1ae">

Wait for the Jenkins to Install suggested plugins

<img width="1291" alt="215959398-344b5721-28ec-47a5-8908-b698e435608d" src="https://github.com/veerarakesh56/Jenkins-Projects/assets/171412850/1ebc2b07-bef5-41ff-8492-46b44e94fe8a">

Create First Admin User or Skip the step [If you want to use this Jenkins instance for future use-cases as well, better to create admin user]

<img width="990" alt="215959757-403246c8-e739-4103-9265-6bdab418013e" src="https://github.com/veerarakesh56/Jenkins-Projects/assets/171412850/73efb9da-5acd-4b89-8bc4-c2c55830e507">

Jenkins Installation is Successful. You can now starting using the Jenkins 

<img width="990" alt="215961440-3f13f82b-61a2-4117-88bc-0da265a67fa7" src="https://github.com/veerarakesh56/Jenkins-Projects/assets/171412850/684e27d4-05a2-4a49-aaa3-8e9abd06291c">

## Install the Docker Pipeline plugin in Jenkins:

   - Log in to Jenkins.
   - Go to Manage Jenkins > Manage Plugins.
   - In the Available tab, search for "Docker Pipeline".
   - Select the plugin and click the Install button.
   - Restart Jenkins after the plugin is installed.

<img width="1392" alt="215973898-7c366525-15db-4876-bd71-49522ecb267d" src="https://github.com/veerarakesh56/Jenkins-Projects/assets/171412850/459b26fc-e98c-45c0-bcf7-877b0466fee7">

Wait for the Jenkins to be restarted.


## Docker Slave Configuration

Run the below command to Install Docker

```
sudo apt update
sudo apt install docker.io
```
 
### Grant Jenkins user and Ubuntu user permission to docker deamon.

```
sudo su - 
usermod -aG docker jenkins
usermod -aG docker ubuntu
systemctl restart docker
```

Once you are done with the above steps, it is better to restart Jenkins.

```
http://<ec2-instance-public-ip>:8080/restart
```

The docker agent configuration is now successful.


