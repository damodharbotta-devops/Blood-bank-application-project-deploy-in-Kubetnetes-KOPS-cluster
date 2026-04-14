   # Blood-bank-application-project-deploy-in-Kubetnetes-KOPS-cluster

# Client Requirement: Client required blood bank application. 

1.	Inside application user can able to see all the information related to application.
2.	New user can ably donate the blood and new user want blood; he/she can able to get the donor blood details.
3.	New user account creation & existing user sign in option.

# DevOps Engineers:

1.	DevOps Engineers build the CI pipeline: Code + CQA + Create Docker image + scan the image + tag and push the image into Docker Hub + Run the application in Docker containers + Migrate and run in Kubernetes.
Note: It is a PHP application. So, there is no Build & test stage in Jenkins pipeline stage.

# Technical stack: 
CI/CD – Jenkins
Create Images – Docker
Deploy and run application – Kubernetes
Monitor – Prometheus & Grafana
Code quality Analysis – Sonar
Scan the Image – Trivy
Cluster – Kops cluster

# Step: 1: Launch an Ec2 instance and install Git, Jenkins, Docker, SonarQube, Trivy in the server.

Server configuration: M7i-Flex.large, EBS volume: 28 Gb storage, Keypair, Security Group – Allow all traffic, Attach IAM role with Full access.

<img width="1917" height="906" alt="Screenshot 2026-04-11 105908" src="https://github.com/user-attachments/assets/2a7954c5-aed8-4324-a038-2ba361a59812" />

•	Install Docker, Git and download SonarQube image in the server. 

<img width="1851" height="375" alt="Screenshot 2026-04-11 110239" src="https://github.com/user-attachments/assets/51c041af-f4a9-459a-a3ba-8311ffbf7388" />
<img width="747" height="804" alt="Screenshot 2026-04-11 110341" src="https://github.com/user-attachments/assets/41886dda-13e6-4b1a-9168-8d81b2b10eab" />
<img width="1187" height="501" alt="Screenshot 2026-04-11 120540" src="https://github.com/user-attachments/assets/e6340aea-8a85-4c3f-aefd-c352b80335ea" />

 
•	Access the Jenkins dashboard using Server public Ip address:8080

<img width="1909" height="937" alt="Screenshot 2026-04-11 113032" src="https://github.com/user-attachments/assets/cd24e620-31a7-496e-b5be-e03481f1e135" />

•	Install required plugins.
       1.	Pipeline stage View
       
 <img width="1912" height="649" alt="Screenshot 2026-04-11 113720" src="https://github.com/user-attachments/assets/9bca4343-99fc-484d-8703-a7b38eaa47af" />

# Stage 1: CODE
•	Generate pipeline syntax and copy the Git command and paste in Jenkins pipeline

<img width="1904" height="937" alt="Screenshot 2026-04-11 114613" src="https://github.com/user-attachments/assets/bfbff1ae-1152-4b9e-b8c5-bb16e5b7139b" />
<img width="1918" height="928" alt="Screenshot 2026-04-11 114755" src="https://github.com/user-attachments/assets/1b34fc69-0375-479d-a69e-1dc80d2fcf21" />

•	Click on build now. Code come in to CI server. We can see in pipeline stage view.

<img width="1906" height="817" alt="Screenshot 2026-04-11 115512" src="https://github.com/user-attachments/assets/596d9a00-aac3-4ba9-ae38-400f61edae8e" />

# Stage: 2: Code Quality Analysis
After install Sonar image in server and access the sonar Qube dash board server public ip address:9000. See below

<img width="1894" height="852" alt="Screenshot 2026-04-11 121935" src="https://github.com/user-attachments/assets/b700fa1a-ae7b-4d35-99ac-787190eb2e4b" />

•	Create a project in SonarQube and fill mandatory fields and generate the syntax.

<img width="1870" height="704" alt="Screenshot 2026-04-11 122102" src="https://github.com/user-attachments/assets/f25af2f5-abaf-421e-90f9-bbf72231a1b2" />
<img width="1919" height="822" alt="Screenshot 2026-04-11 122251" src="https://github.com/user-attachments/assets/f09a82d8-a13c-4249-baa0-f2146c97ff98" />
<img width="1915" height="919" alt="Screenshot 2026-04-11 122433" src="https://github.com/user-attachments/assets/0d59c54c-99a0-44d5-baef-b5e4cbfb54c9" />

•	Copy the generated script (few lines only) and use in pipeline.
•  Now integrate the SonarQube in Jenkins. Install required plugins and integrate with Jenkins in manage Jenkins by adding SonarQube credentials.
               Jenkins -> manage Jenkins -> Plugins -> Available plugins -> Install SonarQube Scanner
               
<img width="1859" height="427" alt="Screenshot 2026-04-11 123209" src="https://github.com/user-attachments/assets/0fef3e68-3c00-4f2c-b3ac-0dd5cc335f5b" />

           Jenkins -> Manage Jenkins -> system -> SonarQube installations
<img width="1873" height="946" alt="Screenshot 2026-04-11 124413" src="https://github.com/user-attachments/assets/4ffd6066-17dd-4b5f-af32-3ab8b9989323" />           

<img width="1893" height="714" alt="Screenshot 2026-04-11 123956" src="https://github.com/user-attachments/assets/bbcc4de4-19b6-46b9-8bce-d12a3209578d" />

•	In SonarQube generate token and copy and paste the token value in SonarQube credentials.
<img width="1893" height="714" alt="Screenshot 2026-04-11 123956" src="https://github.com/user-attachments/assets/89b16918-458c-4664-8b86-649240433210" />
<img width="1896" height="948" alt="Screenshot 2026-04-11 124240" src="https://github.com/user-attachments/assets/1dd4d496-eee4-4654-87bc-9bdf57e4856e" />

•	Now integrate in tools as well.
             Jenkins -> Manage Jenkins -> system -> SonarQube scanner installations
             
 <img width="1904" height="958" alt="Screenshot 2026-04-11 124722" src="https://github.com/user-attachments/assets/cbcaa25e-e36b-4f6c-952c-299ef63d3439" />

 •	Add the CQA syntax in pipeline stage 2 and click on Build now.
 
 <img width="1915" height="932" alt="Screenshot 2026-04-11 132144" src="https://github.com/user-attachments/assets/0e3e1b14-f09c-4502-8ae5-9a491a75fd8d" />

 •	Code quality analysis stage executed successfully.
 
 <img width="1891" height="924" alt="Screenshot 2026-04-11 132528" src="https://github.com/user-attachments/assets/102c4af0-8361-4325-99ea-1dd4a4bf46f4" />

 •	Now check in SonarQube – our project passes the code quality analysis. We have bugs in code; these bugs will take care by developer.
 
 <img width="1904" height="940" alt="Screenshot 2026-04-11 132841" src="https://github.com/user-attachments/assets/a54eb8d1-b26e-4a46-be38-b4d7286c679a" />

# Stage 3: Quality Gates

Quality gates: In stage 2 we checked the code quality analysis, if code get failed, in next stage we should not create docker image with this code. So that in stage 3 we integrate quality gates in pipeline.
•	We should use webhooks in SonarQube otherwise the quality gate stage will be in paused state.
•	Goto SonarQube -> myproject -> project settings -> webhooks -> Create
Name: Jenkins-integration
URL: Jenkins url/sonarqube/webhook/

<img width="1909" height="860" alt="Screenshot 2026-04-11 134727" src="https://github.com/user-attachments/assets/09e3b3c1-e7f5-4116-b0bf-df9ac05b7c90" />

•	Because of this webhooks only quality gates should enable otherwise quality gates should not work.
•	Now in click on pipeline syntax select wait for quality gate and SonarQube password and click on generate pipeline script

<img width="1901" height="958" alt="Screenshot 2026-04-11 135818" src="https://github.com/user-attachments/assets/5e8cc6b4-701a-4964-9d6c-ca383258844f" />

•	Now copy the pipeline syntax and paste in stage 3: Quality gates.

<img width="1919" height="943" alt="Screenshot 2026-04-11 140437" src="https://github.com/user-attachments/assets/a5fb765f-f35e-4bb8-8ede-843d0611ed15" />

•	In quality gates stage it will pause 2 seconds to take response from code quality analysis, based on that quality gate stage will execute.
•	In quality gates we have inbuilt options, all these options should be passes otherwise CI/CD pipeline will be failed.

<img width="1895" height="937" alt="Screenshot 2026-04-11 143330" src="https://github.com/user-attachments/assets/b943558f-6291-4c48-8db3-24fb52f0bb31" />

# Stage 4: Docker images
Now build the docker images. Run the commands to build the database and app images.Docker files are already available in respective folders
.
<img width="1911" height="943" alt="Screenshot 2026-04-11 144646" src="https://github.com/user-attachments/assets/946f84d9-14d8-4b7c-8f8b-89a1656e2e36" />

•	Here we will get error because docker do not have full permissions. Apply this command to get the docker full permissions
     chmod 777 /var/run/docker.sock
     
<img width="1901" height="300" alt="Screenshot 2026-04-11 145012" src="https://github.com/user-attachments/assets/26a8319d-b2e7-4b32-aeab-ac6e4e1c2bd1" />

•	Click on build now, pipeline will run and docker images created.

<img width="1899" height="832" alt="Screenshot 2026-04-11 145559" src="https://github.com/user-attachments/assets/19a59316-354f-44b9-8244-8bb0d9b60a4d" />

# Stage 5: Trivy image 
•	Now scan the docker images using security tool Trivy. Now install Trivy in our server.

<img width="1840" height="199" alt="Screenshot 2026-04-11 150643" src="https://github.com/user-attachments/assets/5d59049f-1ed0-47ca-9ec3-0d5383d04ef8" />

•	Add syntax in stage 5 and run the pipeline it will scan the images.

<img width="1894" height="938" alt="Screenshot 2026-04-11 150602" src="https://github.com/user-attachments/assets/92eb4d22-9225-4a07-a7d9-d9cd896ae141" />

# Step 6: Image tag and push
In this stage tag the both docker images and push into docker hub registry. To push the images into docker Hub registry we need to integrate Jenkins with Docker. For that
install required plugins and add docker Hub credentials in Jenkins CI server.
                                Plugins: Docker Hub pipeline
                                
 <img width="1900" height="613" alt="Screenshot 2026-04-11 152755" src="https://github.com/user-attachments/assets/374e57e4-d386-4698-8dab-00fe8c861acb" />

 •	Now go to pipe line syntax and select docker plugin and docker hub credentials and generate the script. Use the same script in image push stage in pipeline.
 
 <img width="1919" height="946" alt="Screenshot 2026-04-11 154026" src="https://github.com/user-attachments/assets/e1e19258-c38d-42aa-b470-d472763047bd" />

 •	We can see both the images push/upload in to docker Hub.
 
 <img width="1910" height="896" alt="Screenshot 2026-04-11 163801" src="https://github.com/user-attachments/assets/4f9ee260-c201-4feb-916c-7f0c69d716ae" />

# Stage: 7: Deploy into containers
•	Now test this application whether it is working or not. For that we should create containers later we migrate this application into Kubernetes.

  sh 'Docker run -d --name mysqldb -p 3306:3306 damodharbotta/bloodapp:dbimage'
  sh 'Docker run -d --name appcont -p 1111:80 --link mysqldb:msqlcont damodharbotta/bloodapp:appimage'
•	In first command having mysqldb container, this is in sent in Source code and in secondcommand link the db container into app container.

<img width="1913" height="928" alt="Screenshot 2026-04-11 171227" src="https://github.com/user-attachments/assets/71c914ad-8df7-4202-b207-f63b26ca1231" />

•	Click on build now. Pipeline will trigger and docker containers created and app deploy into containers.

<img width="1919" height="623" alt="Screenshot 2026-04-11 171331" src="https://github.com/user-attachments/assets/d03ad441-ff68-4155-8ef3-e932c1f73a2e" />

=>	Application deploy into containers is successful. Now access the application.
=>	Now access the application Public Ip address:1111
        
<img width="1919" height="927" alt="Screenshot 2026-04-11 171810" src="https://github.com/user-attachments/assets/4402fce3-0e66-4a2e-b44b-103f31221e59" />
<img width="1919" height="934" alt="Screenshot 2026-04-11 171930" src="https://github.com/user-attachments/assets/07b65661-c4e5-4119-8e98-06a6c95d37e5" />
        
         






                               
                                














            













      
       





