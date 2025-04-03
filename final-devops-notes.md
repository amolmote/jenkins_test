Welcome to Adv DevOps Training
====
Short url for codeshare: https://shorturl.at/Nk8IY
Lab URL:	https://virtuallabs.palmeto.co.in/	

S. No.	Participant Name	        Username	    Password
1	Viswanadh Velamuri	            BOA-Devops01	P@lmeto@123
2	Samridhi Jain	                BOA-Devops02	P@lmeto@123
3	Mohanan, Vinisha	            BOA-Devops03	P@lmeto@123
4	Nandagopal, Vishnu Priya	    BOA-Devops04	P@lmeto@123
5	Gupta, Pramod K	                BOA-Devops05	P@lmeto@123
6	Shenoy, Amit	                BOA-Devops06	P@lmeto@123
7	Mohanraj, Karthikeyan	        BOA-Devops07	P@lmeto@123
8	Narware, Ashwinee	            BOA-Devops08	P@lmeto@123
9	T, Vignesh	                    BOA-Devops09	P@lmeto@123
10	Gandi, Mahesh	                BOA-Devops10	P@lmeto@123
11	Saxena, Siddhartha	            BOA-Devops11	P@lmeto@123
12	Jaglan, Ritu	                BOA-Devops12	P@lmeto@123
13	Betgerikar, Alok	            BOA-Devops13	P@lmeto@123
14	Kumar, Rajesh2	                BOA-Devops14	P@lmeto@123
15	Mahapatra, Subhadarshini	    BOA-Devops15	P@lmeto@123
16	Aayush Jaiswal	                BOA-Devops16	P@lmeto@123
17	Amol Mote	                    BOA-Devops17	P@lmeto@123
18	Nikhita Kalamkar	            BOA-Devops18	P@lmeto@123
19	Kashish Nanda	                BOA-Devops19	P@lmeto@123
20	Vishvarajsinh Chauhan	        BOA-Devops20	P@lmeto@123
21	Adweteey Dwivedi	            BOA-Devops21	P@lmeto@123
22	Jegannthan	                    BOA-Devops22	P@lmeto@123
23	Sampath, Marappan	            BOA-Devops23	P@lmeto@123
24	Ramanathan, Keerthana	        BOA-Devops24	P@lmeto@123
25	Sudheer, Bhogu Venkata Subbarama	BOA-Devops25	P@lmeto@123
26	Vimal, Sneha 	                BOA-Devops26	P@lmeto@123
27	Anandharaju, Prakash 	        BOA-Devops27	P@lmeto@123

Pre Assessment:
https://forms.office.com/r/GFFq1CAvdk

Power on lab server
---
vmware workstation -- 
    ubuntu-01 -- power on
    ubuntu-02 -- power on
    server-19 -- power on
    
Connect to ubunto-01 via putty
---
open VM Login Details.txt file in your lab server desktop and get the ip/username/password

connect via putty

install and configure jenkins
---
install latest patches
---
sudo su
apt update
apt upgrade -y

install java 21
---
apt install openjdk-21-jdk -y

install jenkins -- https://www.jenkins.io/doc/book/installing/linux/#debianubuntu
--
wget -O /usr/share/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" https://pkg.jenkins.io/debian-stable binary/ | tee /etc/apt/sources.list.d/jenkins.list > /dev/null
apt update
apt install jenkins -y

access jenkins
---
http://{your ubuntu01 ip}:8080/

get inial admin password
---
cat /var/lib/jenkins/secrets/initialAdminPassword

Configure jenkins
---
password from previous command

Customize Jenkins page -- select "install suggested plugins"

Create First Admin User:
    username: admin
    Password: admin
    Confirm password: admin
    Full name: Adminiustrator
    E-mail address: admin@admin.com
    
save and continue

save and finish

start using jenkins

Login to your github account and fork the repo
---
https://github.com/sathishbob/jenkins_test

generate a key pair on jenkins
--
ssh-keygen

Enter file in which to save the key (/root/.ssh/id_rsa): -- Press Enter
Enter passphrase (empty for no passphrase): -- Press Enter
Enter same passphrase again: -- Press Enter

copy the contenet of public key
---
cat /root/.ssh/id_rsa.pub

Add the public key to the user level in git hub
---
user profile icon -- settings-- ssh and gpg keys -- new ssh key
title -- jenkins
key -- public key content
ADD ssh key

add the private key to the jenkins
---
dashboard -- manage jenkins -- credentials -- system -- global credentials -- add credentials 
kind : ssh username with private key
    ID : github
    Username: git
    Private Key: select "enter directlry"
		add
		paster the output of command  - cat /root/.ssh/id_rsa 
Create

install git on jenkins
---
apt install git -y

Get repo url
---
github -- repo -- code -- ssh

Create a jenkins job to pull source code
---
dashboard -- New item
Enter an item name : javaapp
select -- freestyle project
source code management -- select git
    Repository URL -- repo ssh url (git@github.com:{your github id}/jenkins_test.git)
    Credentials -- select "git"
save

Change the git ssh option
---
Dashboard -- manage jenkins -- security
Git Host Key Verification Configuration
    Host Key Verification Strategy -- accept first connection
save

Check error is gone on job source code management section
---
Dashboard -- javaapp -- Configuration -- source code management

Run the job
--
Dashboard -- javaapp -- build now

Install maven in global tools 
---
dashboard -- manage jenkins -- tools -- Maven installations -- Add Maven

Name: MVN3
version: 3.9.9

Name: MVN2
version: 2.2.1

Save

Add build step to the job
---
Dashboard -- javaapp -- Configuration -- Build Steps -- Add Build step
    Invoke top-level Maven targets
    Maven Version : MVN3
    Goals: clean package
    Advanced:
        POM : api-gateway/
save 

Build Now

Add post build steop to archive the jar file and test report
---
Dashboard -- javaapp -- Configuration -- Build Steps -- Add Post-build Actions
    Archive the artifacts 
    Files to archive: api-gateway/target/*.jar
    
    Publish JUnit test result report
    Test report XMLs :  api-gateway/target/surefire-reports/*.xml
    
Create new branches in git hub
---
git hub -- your repo -- branches -- new brach
    dev
    test
    
Modify the job to allow the user to input the branch
---
Dashboard -- javaapp -- Configuration -- General -- select "This project is parameterized" -- Add parameters
    String parameter
        Name: BRANCH
        Default Value: master
        Description: Please enter the branch name to build
        
update the source code managenet to use the variable insted of master codes value "master"
    Source code management -- git
        Branches to build
            Branch Specifier (blank for 'any') 
                */${BRANCH}
save

Build with parameter-- dev

Build

Day 2
====
Power on lab server
---
vmware workstation -- 
    ubuntu-01 -- power on
    ubuntu-02 -- power on
    server-19 -- power on
    
access jenkins
---
http://{your ubuntu01 ip}:8080/

Modify the job to change parameter type from string to choice
---
Dashboard -- javaapp -- Configuration -- General -- select "This project is parameterized" -- delete string parameter
Add parameters -- Choice Parameter
        Name: BRANCH
        Choices: 
            master
            dev
            test
        Description: 1

save

Build with parameter-- test


Install git parameter plugin
---
Dashboard -- Manage Jenkins -- Plugins -- Avaliable plugins
Git Parameter

select and install

Select "Restart Jenkins when installation is complete and no jobs are running"

Modify the job to change parameter type from choice to git
---
Dashboard -- javaapp -- Configuration -- General -- select "This project is parameterized" -- delete string parameter
Add parameters -- git Parameter
        Name: BRANCH
        Description: Please select the branch name to build
        Parameter Type: Bramch
        Default Value: origin/master
        
update the source code managenet to use the variable without path
    Source code management -- git
        Branches to build
            Branch Specifier (blank for 'any') 
                ${BRANCH}
                
save


Create new branches in git hub
---
git hub -- your repo -- branches -- new branch
    qa
    

Build with parameter-- origin/qa

Cron expression
--
* 	* 	* 	* 	*
Minutes
	hour
		day
			month
				Day of the week

Every day at 10 AM - 0 10 * * *
Every day at 10 PM - 0 22 * * *
Every weekday at 10 PM - 0 22 * * 1,2,3,4,5
Every weekday at 10 PM - 0 22 * * 1-5
every month first day 10 am - 0 10 1 * *
every quarter first day 10 am - 0 10 1 1,3,6,9 *
every quarter first day 10 am - 0 10 1 */4 *
every 2 minutes - */2 * * * *

Modify the job to run every 2 minutes
---
Dashboard -- javaapp -- Configuration -- triggeres

Select Build periodically
*/2 * * * *


Modify the job to run every 2 minutes butr create build only when a change is avaliable in repo
---
Dashboard -- javaapp -- Configuration -- triggeres

uncheck Build periodically

Select poll scm
*/2 * * * *


you will see a new option git polling log

Add a commit to repo and see whether the job is triggerd


Create a new pipeline job
---
dashboard -- new item -- javapipeline -- pipeline -- create

pipeline:

v  

save 

Build now

To view the pipeline graph
---
Dashboard -- Manage Jenkins -- Appearance
Pipeline graph view -- select
    Show pipeline graph on job page
    Show pipeline graph on build page
save

Rewrite the pipeline to have multiple stages
---
pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "MVN3"
    }

    stages {
        stage('pull scm') {
            steps {
                // Get some code from a GitHub repository
                git credentialsId: 'github', url: 'git@github.com:{your github id}/jenkins_test.git'
            }
        }
        
        stage('Build') {
            steps {
                sh "mvn -Dmaven.test.failure.ignore=true -f api-gateway/ clean package"
            }
                            
        }
        
        stage('archive') {
            steps {
                archiveArtifacts artifacts: 'api-gateway/target/*.jar', followSymlinks: false
            }
        }
        
        stage('publish test result') {
            steps {
                junit 'api-gateway/target/surefire-reports/*.xml'
            }
        }
    }
}

Modify the pipeline to get the pipeline from repo
---
dashboard -- javapipeline -- configure -- pipeline
Definition : Pipeline script form SCM
    SCM : git
        Repositories -- Repository URL
        credentails -- git
Script Path : Jenkinsfile

triggers
    Select poll scm
        * * * * *
        
save

In the repo update jenkins file
---
git hub -- your repo -- Jenkins file


pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "MVN3"
    }

    stages {
        stage('pull scm') {
            steps {
                // Get some code from a GitHub repository
                git credentialsId: 'github', url: 'git@github.com:{your github id}/jenkins_test.git'
            }
        }
        
        stage('Build') {
            steps {
                sh "mvn -Dmaven.test.failure.ignore=true -f api-gateway/ clean package"
            }
                            
        }
        
        stage('archive') {
            steps {
                archiveArtifacts artifacts: 'api-gateway/target/*.jar', followSymlinks: false
            }
        }
        
        stage('publish test result') {
            steps {
                junit 'api-gateway/target/surefire-reports/*.xml'
            }
        }
    }
}

commit changes

Modify the jenkins file in repo to add a new stage 
---

pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "MVN3"
    }

    stages {
        stage('pull scm') {
            steps {
                // Get some code from a GitHub repository
                git credentialsId: 'github', url: 'git@github.com:{your github id}/jenkins_test.git'
            }
        }
        
        stage('Build') {
            steps {
                sh "mvn -Dmaven.test.failure.ignore=true -f api-gateway/ clean package"
            }
                    
        }
        
        stage('archive') {
            steps {
                archiveArtifacts artifacts: 'api-gateway/target/*.jar', followSymlinks: false
            }
        }
        
        stage('publish test result') {
            steps {
                junit 'api-gateway/target/surefire-reports/*.xml'
            }
        }
        
        stage('test') {
            steps {
                sh "echo testing"
            }
        }
    }
}

Jenkins pipeline documentation
---
https://www.jenkins.io/doc/book/pipeline/

Create a new pipeline job
---
dashboard -- new item -- parameter -- pipeline -- create

pipeline:

pipeline {
    agent any
    
    stages{
        stage("setup parameter") {
            steps {
                script {
                    properties([
                        parameters([
                            choice(
                                choices: ["dev", "uat", "prod"],
                                name: "ENVIRONMENT"
                            ),
                            string(
                                defaultValue: "training",
                                name: "STRING"
                            )
                        ])
                    ])
                }
            }
        }
        
        stage("print parameter") {
            steps {
                echo "choice parameter is $ENVIRONMENT"
                echo "string parameter is $STRING"
            }
        }
    }
}

save

build

It will be a failure

Build with parameter

Modifyt the pipeline to add triggeres
---

pipeline {
    agent any
    
    triggers {
        cron("*/5 * * * *")
    }
    
    stages{
        stage("setup parameter") {
            steps {
                script {
                    properties([
                        parameters([
                            choice(
                                choices: ["dev", "uat", "prod"],
                                name: "ENVIRONMENT"
                            ),
                            string(
                                defaultValue: "training",
                                name: "STRING"
                            )
                        ])
                    ])
                }
            }
        }
        
        stage("print parameter") {
            steps {
                echo "choice parameter is $ENVIRONMENT"
                echo "string parameter is $STRING"
            }
        }
    }
}

Connect to ubuntu-02 node via putty
---
Connect to ubunto-02 via putty
---
open VM Login Details.txt file in your lab server desktop and get the ip/username/password

connect via putty

sudo su

apt update

install java 21
---
apt install openjdk-21-jdk -y

Install git
---
apt install git -y

Add the node in master
---
Dashboard -- managejenkins -- nodes -- new node

Node name : linux
    select "Permenant agent"
Create

Number of executors: 2
Remote root directory: /home/palmeto
Labels: linux
Usage: "Only build jobs with label expression matching this node"
Launch method: "Launch agent via ssh"
Host: {ubuntu 2 ip address}
Credentials:
    Add
       kind: User name with password
       Username: palmeto
       password: palmeto
       ID: linux
      ADD
    Select palmeto from list
Host Key Verification Strategy : Non verifying verification statergy
save

Check linux is in sync

Configure a job to run on a node
--
Dashboard -- javaapp -- Configuration -- general
select "Restrict where this project can be run"
Label Expression: linux

save

build with parameter

Update the pipeline to run the job on linux node
----

pipeline {
    agent {
        label 'linux'
    }

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "MVN3"
    }

    stages {
        stage('pull scm') {
            steps {
                // Get some code from a GitHub repository
                git credentialsId: 'github', url: 'git@github.com:{your github id}/jenkins_test.git'
            }
        }
        
        stage('Build') {
            steps {
                sh "mvn -Dmaven.test.failure.ignore=true -f api-gateway/ clean package"
            }
                            
        }
        
        stage('archive') {
            steps {
                archiveArtifacts artifacts: 'api-gateway/target/*.jar', followSymlinks: false
            }
        }
        
        stage('publish test result') {
            steps {
                junit 'api-gateway/target/surefire-reports/*.xml'
            }
        }
        
        stage('test') {
            steps {
                sh "echo testing"
            }
        }
    }
}

Update the pipeline to run a particular stage on a node
---
pipeline {
    agent any
    
    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "MVN3"
    }

    stages {
        stage('pull scm') {
            steps {
                // Get some code from a GitHub repository
                git credentialsId: 'github', url: 'git@github.com:{your github id}/jenkins_test.git'
            }
        }
        
        stage('Build') {
            steps {
                sh "mvn -Dmaven.test.failure.ignore=true -f api-gateway/ clean package"
            }
                            
        }
        
        stage('archive') {
            steps {
                archiveArtifacts artifacts: 'api-gateway/target/*.jar', followSymlinks: false
            }
        }
        
        stage('publish test result') {
            steps {
                junit 'api-gateway/target/surefire-reports/*.xml'
            }
        }
        
        stage('test') {
            agent {
                label 'linux'
            }
            steps {
                sh "echo testing"
            }
        }
    }
}

Login to windows node in vmware
---
vmware -- server 19
    vm -- send "ctrl+ alt+ del"

Login with password

Install jdk
---
https://download.oracle.com/java/21/latest/jdk-21_windows-x64_bin.exe

Install git bash
---
https://github.com/git-for-windows/git/releases/download/v2.49.0.windows.1/Git-2.49.0-64-bit.exe

Enable tcp port for inbound agent
--
dashboard -- mange -- security -- Agents
TCP port for inbound agents -- select "Fixed"
50000

save

Add the node in master
---
Dashboard -- managejenkins -- nodes -- new node

Node name : windows
    select "Permenant agent"
Create

Number of executors: 2
Remote root directory: C:\jenkins
Labels: windows
Usage: "Only build jobs with label expression matching this node"
Launch method: "Launch agent by connecting it to the controller"
save


click on windows node

Copy the command below "Run from agent command line: (Windows) "


open command prompt as administrator in windows node and pater the copied command
---
sample:
curl.exe -sO http://192.168.0.201:8080/jnlpJars/agent.jar
java -jar agent.jar -url http://192.168.0.201:8080/ -secret 6b5ba35e8be7d64b0076f96350dbbdaa29edd280d78f6542e78925c9d3896179 -name windows -webSocket -workDir "C:\jenkins"


Check node is in sync
---
Dashboard -- managejenkins -- nodes 

Configure a job to run on a windows node
--
Dashboard -- javaapp -- Configuration -- general
select "Restrict where this project can be run"
Label Expression: windows

save

build with parameter

Variables in pipeline
---

dashboard -- new item -- variable -- pipeline -- create

pipeline {
    agent any
    
    environment {
        TRAINING = "devops"
        TOPIC = "jenkins"
    }
    
    stages {
        stage('local') {
            environment {
                TOPIC = "cicd"
            }
            
            steps {
                sh "echo training is ${TRAINING} and topic is ${TOPIC}"
            }
        }
        
        stage('global') {
            steps {
                sh "echo training is ${TRAINING} and topic is ${TOPIC}"
            }
        }
    }
}

save

build

Condition in pipeline
---

dashboard -- new item -- Condition -- pipeline -- create

pipeline {
    agent any
    environment {
        BUILD = "${currentBuild.getNumber() % 2}"
    }
    
    stages {
        stage("normal") {
            steps {
                echo "job with condition"
            }
        }
        
        stage("condition") {
            when{
                environment name: "BUILD", value: "0"
            }
            steps  {
                echo "build number is even"
            }
        }
    }
}

save

build

loop in pipeline
---

dashboard -- new item -- Condition -- pipeline -- create

def map = [training: "devops", topic: "jenkins", lab: "onprem" ]

pipeline {
    agent any
    stages {
        stage("loop") {
            steps {
                script {
                    map.each { entry ->
                    stage(entry.key) {
                        echo "$entry.value"
                    }
                    }
                }
            }
        }
    }
}

save

build

Parallel in pipeline
----

dashboard -- new item -- parallel -- pipeline -- create

pipeline {
    agent any
    
    stages {
        stage("normal") {
            steps {
                echo "I am running in sequence"
            }
        }
        
        stage("parallel") {
            parallel {
                stage("firefox") {
                    steps {
                        echo "I am testing in firefox"
                    }
                }
                
                stage("safari") {
                    steps {
                        echo "I am testing in safari"
                    }
                }
                
                stage("chrome") {
                    steps {
                        echo "I am testing in chrome"
                    }
                }
                
                stage("edge") {
                    steps {
                        echo "I am testing in edge"
                    }
                }
            }
        }
    }
}

save

build

Approval in pipeline
---
dashboard -- new item -- approval -- pipeline -- create

pipeline {
    agent any
    
    stages {
        stage("pull") {
            steps {
                echo "pulling code"
            }
        }
        
        stage("build") {
            steps {
                echo "building code"
            }
        }
        
        stage("approval") {
            steps {
                input "Please approve to proceed with deployment"
            }
        }
        
        stage ("deployment") {
            steps {
                echo "deploying application"
            }
        }
    }
}

save

build

in the build 1 -- click "paused for input" -- click proceed


build

in the build 2 -- click "paused for input" -- click abort

Modify the job to have a time out for approval process
---

dashboard -- approval -- configure -- pipeline

pipeline {
    agent any
    
    stages {
        stage("pull") {
            steps {
                echo "pulling code"
            }
        }
        
        stage("build") {
            steps {
                echo "building code"
            }
        }
        
        stage("approval") {
            options {
                timeout(time:1, unit: 'MINUTES')
            }
            steps {
                input "Please approve to proceed with deployment"
            }
        }
        
        stage ("deployment") {
            steps {
                echo "deploying application"
            }
        }
    }
}

build

dont provide approval, it will exit after a minute
.
build

in the new build  -- click "paused for input" -- click proceed

Day 3
====
Power on lab server
---
vmware workstation -- 
    ubuntu-01 -- power on
    
connect via putty and Switch to root user
---
sudo su

install docker
---
Add Docker's official GPG key:
---
apt update
apt install ca-certificates curl -y
install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
chmod a+r /etc/apt/keyrings/docker.asc

Add the repository to Apt sources:
---
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
apt-get update

install docker
---
apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y

check docker installtion
---
docker ps

List the images in the server
--
docker images

run a hello world container
---
docker run hello-world

list the images
---
docker images

run a hello world container again
---
docker run hello-world

list the running container
---
docker ps

list all containers
---
docker ps -a

remove a exitied container
--
docker rm {your container id}
docker rm {your container name}

remove a image
---
docker rmi hello-world

run a ngnix container
---
docker run nginx

CTRL + C

run the container in daemon mode
---
docker run -d nginx

stop a running container
--
docker stop {your container id}

remove all exited containers
---
docker rm {your container id 1} {your container id 2}

option to remove the container after exit
---
docker run -d --rm nginx

stop a running container
---
docker stop {your container id}

check the exited container
---
docker ps -a

run a conatiner with port mapping
---
docker run -d --rm -p 80:80 nginx

Access ubuntu01 ip via browser
---
http://{ubunt01 ip}

stop the container
---
docker stop {your container id}

run the container with custom name
---
docker run -d --rm --name web -p 80:80 nginx

get the logs of running container
--
docker logs web

follow the logs of container
---
docker logs -f web
 
to exit Ctrl + c

take a teriminal inside the container
---
docker exec -it web bash

modify the index.html inside the container
---
echo "Welcome to docker training" > /usr/share/nginx/html/index.html

Come out of teriminal
---
exit

run a command inside the container
--
docker exec web ls -l
docker exec web cat /usr/share/nginx/html/index.html

get the real time stats
---
docker stats web

command to list running process
---
docker top web

Docker cli documentation
---
https://docs.docker.com/reference/cli/docker/

switch to home directory of root user
---
cd

create a directory for firstdocker
---
mkdir firstdocker
cd 

Install latest version of vi editor
---
apt install vim -y

Create a dockerfile
---
vi Dockerfile

Docker file content
---

#Define base image
FROM ubuntu:22.04

MAINTAINER sathish
LABEL owner.email="sathishbob@gmail.com"

# install apache package
RUN apt update
RUN apt install apache2 -y

# Craete a static webpage
RUN echo "Welcome to docker training" > /var/www/html/index.html

# Create a script to start the apache in foreground
RUN echo ". /etc/apache2/envvars" > /run.sh
RUN echo "mkdir -p /var/run/apache2" >> /run.sh
RUN echo "mkdir -p /var/lock/apache2" >> /run.sh
RUN echo "/usr/sbin/apache2 -D FOREGROUND" >> /run.sh

# execute permiussion for the script
RUN chmod +x /run.sh

# Define the port number
EXPOSE 80

CMD /run.sh

buld the docker file
---
docker build -t apache .

list the images
---
docker images

stop the web container
---
docker stop web

run the container
--
docker run -d --rm --name web -p 80:80 apache

Access ubuntu01 ip via browser
---
http://{ubunt01 ip}

Create a account at docker hub
---
https://hub.docker.com/

command to get the layers of a image
---
docker image history apache

login to dockerhub from cli
---
docker login

Follow on screen instruction

try to ush the image apache -- this will fail
---
docker push apache

rename the image with your dockerhub account id in the name
---
docker tag apache {your docker hub id}/apache

push the image
---
docker push {your docker hub id}/apache

stop the container
---
docker kill {your container id}

command to pull a image
---
docker pull ubuntu
 
command to pull ubuntu 22
----
docker pull ubuntu:22.04

command to delete all exitied containers
---
docker container prune

command to delete dangiling images
---
docker image prune

command to delete all the images without at least one container associated to them
---
docker image prune -a

command for system cleanup
--
docker image prune -a

command for deeper system clean
--
docker image prune -ae -a

run a jenkins container
---
docker run -d --rm --name jenkins -p 80:8080 jenkins/jenkins

get the password from the logs
---
docker logs jenkins

Configure jenkins and create a test job

stop the container
---
docker stop jenkins

try to rerun the container
---
docker run -d --rm --name jenkins -p 80:8080 jenkins/jenkins

stop the container
---
docker stop jenkins

command to list volumes
---
docker volume ls

create a volume
---
docker volume create jenkinsdata

Get details of the volume crearred
---
docker inspect volume jenkinsdata

run the container by attaching the volume
---
docker run -d --rm --name jenkins -p 80:8080 -v jenkinsdata:/var/jenkins_home jenkins/jenkins

get the password from the volume(local)
---
cat /var/lib/docker/volumes/jenkinsdata/_data/secrets/initialAdminPassword

stop the container
---
docker stop jenkins

run the container by attaching the volume
---
docker run -d --rm --name jenkins -p 80:8080 -v jenkinsdata:/var/jenkins_home jenkins/jenkins

cwd to root directory
---
cd

Create a diretory for cmd
---
mkdir cmd
vi cmd/Dockerfile

contenet of docker file
----
FROM busybox
CMD ["ping", "-c", "4","www.google.com"]

build and run the container
---
docker build -t cmd cmd/
docker run cmd

try to run the container with argument -- this will error
---
docker run cmd www.microsoft.com

create a directory for entrypoint
---
mkdir entry
vi entry/Dockerfile

contenet of docker file
---
FROM busybox
ENTRYPOINT ["ping", "-c", "4"]

build and run the container
---
docker build -t entry entry/

run the container with argument
---
docker run entry www.google.com
docker run entry www.microsoft.com


Create a directory for entry point and cmd
---
mkdir entrycmd
vi entrycmd/Dockerfile

contenet of docker file
---
FROM busybox
ENTRYPOINT ["ping","-c","4"]
CMD ["www.google.com"]

build and run the container with and without arguments
---
docker build -t entrycmd entrycmd/
docker run entrycmd
docker run entrycmd www.microsoft.com

Create a directory for environment variable
---docker run entrycmd www.microsoft.com
mkdir env
vi env/Dockerfile

contenet of docker file
---
FROM busybox
ENV var1=training var2=docker
CMD ["env"]

build and run the container
---
docker build -t environment env/
docker run environment

pass the environment variable while running the container
---
docker run -e var3=azure environment
docker run -e var3=azure -e var1=test environment

copy diretive
---
mkdir copy
vi copy/Dockerfile

contenet of docker file
---
FROM ubuntu
RUN apt update
RUN apt install apache2 -y
WORKDIR /var/www/html
COPY index.html .
CMD ["ls","-l"]

vi copy/index.html

contenet of index.html
---
<html>
        <body>
                <h1> Welcome to docker training </h1>
        </body>
</html>

docker build -t webapp copy/

docker run webapp

 docker run webapp cat index.html
 
add directive
---
vi copy/Dockerfile

contenet of docker file
---

FROM ubuntu
RUN apt update
RUN apt install apache2 -y
WORKDIR /var/www/
COPY index.html .
CMD ["ls","-l"]

build and run the container
---
docker build -t webapp copy/

docker run webapp

user directive
---
mkdir user

vi user/Dockerfile

contenet of docker file
---
FROM ubuntu
RUN useradd -m training
USER training
CMD ["whoami"]

build and run the container
---
docker build -t user user/

docker run user

run a container without limit
---
docker run -d --name withoutlimit sathishbob/website

get the stats of container
---
docker stats withoutlimit

get the cpu limit
---
docker inspect withoutlimit | grep -i nano

run a container with cpu and memory limit
---
docker run -d --memory="256m" --cpus=0.5 --name withlimit sathishbob/website

get the stats of container
---
docker stats withlimit

get the cpu limit
---
docker inspect withlimit | grep -i nano

Day4
===
command to list the networks
---
docker network ls

Stop all the running containers
---
docker stop withlimit withoutlimit jenkins

Remove all stopped containers
---
docker container prune

run a container on host network
---
docker run -d --rm --name web --network host sathishbob/apache

run a container on default network
---
docker run -it busybox ifconfig

run a container on the host network
---
docker run -it --network host busybox ifconfig

run a container on the none network
---
docker run -it --network none busybox ifconfig

create two networks
---
docker network create networka
docker network create networkb

inspect the network
---
docker network inspect networka
docker network inspect networkb

Stop web container
---
docker stop web

bring web container on networka
---
docker run -itd --network networka --network-alias web --name web alpine

bring db container on networkb
---
docker run -itd --network networkb --network-alias db --name db alpine

bring app container on networka
---
docker run -itd --network networka --network-alias app --name app alpine

attache networkb to app container
---
docker network connect --alias app networkb app

check the ip of all the containers
---
docker exec web ip a
docker exec db ip a
docker exec app ip a

check the connectivity from web
---
docker exec web ping -c 2 app
docker exec web ping -c 2 db ---bad connection

check the connectivity from app
---
docker exec app ping -c 2 web
docker exec app ping -c 2 db

check the connectivity from app
---
docker exec db ping -c 2 app
docker exec db ping -c 2 web --bad connection

docker compose
--
cd 
mkdir dockercompose
cd dockercompose/

create docker compose yaml
---
vi docker-compose.yml

docker compose contene
---
version: '3.8'  # Version of Docker Compose

services:
  web:
    image: nginx:latest  # Nginx web server
    ports:
      - "80:80"  # Map port 80 on the host to port 80 on the container
    volumes:
      - ./html:/usr/share/nginx/html  # Mount local directory for serving static files
    depends_on:
      - db  # Ensure web starts after the database

  db:
    image: mysql:5.7  # MySQL database
    environment:
      MYSQL_ROOT_PASSWORD: examplepassword  # Set root password for MySQL
      MYSQL_DATABASE: exampledb  # Create a database called 'exampledb'
      MYSQL_USER: user  # Create a user
      MYSQL_PASSWORD: userpassword  # Set password for user
    volumes:
      - db_data:/var/lib/mysql  # Persist MySQL data

volumes:
  db_data:  # Named volume for the MySQL database

bring up the docker compose
---
docker compose up

Ctrl + C to come out

bring up the docker container in daemon mode
---
docker compose up -d

Command to list docker compose services
---
docker compose ps

Command to get the logs 
---
docker compose logs
docker compose logs web
docker compose logs db

Command to bring down the services
---
docker compose down


Azure credentaisl
---
URL	Email	Password
https://portal.azure.com	traininguser3@zippyopsgmail.onmicrosoft.com	    8vvH3sIKQh5GSizf -- sathish(trainer)
https://portal.azure.com	traininguser4@zippyopsgmail.onmicrosoft.com	    8vvH3sIKQh5GSizf -- Keerthana
https://portal.azure.com	traininguser5@zippyopsgmail.onmicrosoft.com	    8vvH3sIKQh5GSizf -- Marappan
https://portal.azure.com	traininguser6@zippyopsgmail.onmicrosoft.com	    8vvH3sIKQh5GSizf -- amit
https://portal.azure.com	traininguser7@zippyopsgmail.onmicrosoft.com	    8vvH3sIKQh5GSizf 
https://portal.azure.com	traininguser8@zippyopsgmail.onmicrosoft.com	    8vvH3sIKQh5GSizf-- Karthik
https://portal.azure.com	traininguser9@zippyopsgmail.onmicrosoft.com	    8vvH3sIKQh5GSizf -- Samridhi
https://portal.azure.com	traininguser10@zippyopsgmail.onmicrosoft.com	8vvH3sIKQh5GSizf -- mahesh
https://portal.azure.com	traininguser11@zippyopsgmail.onmicrosoft.com	8vvH3sIKQh5GSizf -- vignesh
https://portal.azure.com	traininguser12@zippyopsgmail.onmicrosoft.com	8vvH3sIKQh5GSizf -- Vinisha
https://portal.azure.com	traininguser13@zippyopsgmail.onmicrosoft.com	8vvH3sIKQh5GSizf -- Alok B
https://portal.azure.com	traininguser14@zippyopsgmail.onmicrosoft.com	8vvH3sIKQh5GSizf -- Vishnu Priya
https://portal.azure.com	traininguser15@zippyopsgmail.onmicrosoft.com	8vvH3sIKQh5GSizf -- Subha
https://portal.azure.com	traininguser16@zippyopsgmail.onmicrosoft.com	8vvH3sIKQh5GSizf
https://portal.azure.com	traininguser17@zippyopsgmail.onmicrosoft.com	8vvH3sIKQh5GSizf -- Amol
https://portal.azure.com	traininguser18@zippyopsgmail.onmicrosoft.com	8vvH3sIKQh5GSizf -- Rajesh
https://portal.azure.com	traininguser19@zippyopsgmail.onmicrosoft.com	8vvH3sIKQh5GSizf --ashwinee
https://portal.azure.com	traininguser20@zippyopsgmail.onmicrosoft.com	8vvH3sIKQh5GSizf
https://portal.azure.com	traininguser21@zippyopsgmail.onmicrosoft.com	8vvH3sIKQh5GSizf
https://portal.azure.com	traininguser22@zippyopsgmail.onmicrosoft.com	8vvH3sIKQh5GSizf
https://portal.azure.com	traininguser23@zippyopsgmail.onmicrosoft.com	8vvH3sIKQh5GSizf
https://portal.azure.com	traininguser24@zippyopsgmail.onmicrosoft.com	8vvH3sIKQh5GSizf -- Nikhita
https://portal.azure.com	traininguser25@zippyopsgmail.onmicrosoft.com	8vvH3sIKQh5GSizf
https://portal.azure.com	traininguser26@zippyopsgmail.onmicrosoft.com	8vvH3sIKQh5GSizf -- Sneha
https://portal.azure.com	traininguser27@zippyopsgmail.onmicrosoft.com	8vvH3sIKQh5GSizf -- Prakash

Create kubernetes cluster
---
azure -- search "Kubernetes services" -- create -- kubernetes cluster
General
    Resource group -- Create new -- Name -- {your name} -- ok
    Cluster details -- Cluster preset configuration -- Dev/Test
    Kubernetes cluster name -- {your name}
    Region -- East US
    Availability zones -- None
    AKS pricing tier -- Free
    Kubernetes version -- 1.30.10
    Authentication and Authorization -- local accounts with kubernetes RBAC
    
    Next

Node Pools
    agent pool
        Node size -- chose a size
            select D2s_v3
        update
        
    next
    
Networking: No changes

    next
    
Integration: No changes

    next
    
Monitoring: No changes

    next
    
Security: No changes

    next
    
Advanced: No changes

    next
    
Tags: No changes

    next
    
Review and create: No changes

    Create
    
Click on "Go to Resources"

Install azure cli and kubectl on the ubuntu 01
---
install azure cli
---
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash

check azure cli installation
---
az --version

login to azure cli
---
az login

follow on screen instruction

Press Enter

download and install kubectl
---
curl -LO https://dl.k8s.io/release/v1.30.10/bin/linux/amd64/kubectl

chmod +x kubectl
mv kubectl /usr/bin/

Get the kubeconfig from azure
-- 
azure -- kubernetes service -- your clustet -- connect
    copy paste the command under "Download cluster credentials"
    
test the connectivity
---
kubectl get nodescommand to get all nodes
---
kubectl get nodes

to get detalled information about a resource
---
kubectl describe node  {your node name}to get namespaces
---
kubectl get namespaces
kubectl get ns

get deployments
---
kubectl get deployment
kubectl get deployment -n kube-system
kubectl get deployment -A
kubectl get deployment --all-namespaces
kubectl get deploy -A

replicaset
---
kubectl get rs -A
kubectl get rs -n kube-system
kubectl get replicaset -n kube-system

pod
--
kubectl get pod -A
kubectl get po  -n kube-system

Service
---
kubectl get service -n kube-system
kubectl get svc -n kube-system

Daemonset
---
kubectl get daemonset -n kube-system
kubectl get ds -n kube-system

statefulset
--
kubectl get statefulset -n kube-system

persistenntvolumeclaim on all namespaces
---
kubectl get pvc -n kube-system
kubectl get persistentvolumeclaim -n kube-system

storageclass
---
kubectl get sc
kubectl get storgeclass

persistant volume
---
kubectl get pv
kubectl get persistantvolume

config map
---
kubectl get cm --all-namespaces
kubectl get configmap --all-namespaces

secrets
---
kubectl get secrets --all-namespaces

Api documentation
---
https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/

create directory for kubernetes
---
cd

mkdir kubernetes
cd kubernetes/

create firtsapp
---

vi firstapp.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: webdeployment
  labels:
    app: web
spec:
  replicas: 2
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
      - name: ngnixpod
        image: nginx
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: webservice
  labels:
    app: web
spec:
  ports:
  - port: 80
    protocol: TCP
  selector:
    app: web
  type: LoadBalancer

create the resources in default namespace
---
kubectl create -f firstapp.yaml

list all resources in default namespace
---
kubectl get all

access the public ip on browser
---
http://{your public ip}

Delete a pod 
---
kubectl delete {any pod}

list all resources in default namespace
---
kubectl get all

Modify the yaml to increase the replicas from 2 to 4
---

vi firstapp.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: webdeployment
  labels:
    app: web
spec:
  replicas: 4
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
      - name: ngnixpod
        image: nginx
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: webservice
  labels:
    app: web
spec:
  ports:
  - port: 80
    protocol: TCP
  selector:
    app: web
  type: LoadBalancer
  
try to run the create again
---
kubectl create -f firstapp.yaml

apply the yaml
---
kubectl apply -f firstapp.yaml

edit deployment
---
kubectl edit deploy webdeployment
change replica from 4 to 2

get the deployment yaml
---
kubectl get deploy webdeployment -o yaml

Delete the resources created with yaml
---
kubectl delete -f firstapp.yaml

apply the yaml
---
kubectl apply -f firstapp.yaml
kubectl get all

manual scalling
---
kubectl scale deploy webdeployment --replicas=5
kubectl scale deploy webdeployment --replicas=2

Autoscale a deployment
---
kubectl autoscale deployment webdeployment --min=2 --max=10 --cpu-percent=80

List the hpa
---
kubectl get hpa

Perform clean up
---
kubectl delete -f firstapp.yaml
kubectl delete hpa webdeployment

Create a blue deployment
---
vi blue.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: bluedeployment
  labels:
    app: web
spec:
  replicas: 4
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
        version: blue
    spec:
      containers:
      - name: nginxpod
        image: nginx:1.26
        ports:
        - containerPort: 80

kubectl apply -f blue.yaml

Create service pointing to blue deployment
---
vi service.yaml

apiVersion: v1
kind: Service
metadata:
  name: webservice
  labels:
    app: web
spec:
  ports:
  - port: 80
    protocol: TCP
  selector:
    app: web
    version: blue
  type: LoadBalancer

kubectl apply -f service.yaml

List the services
---
kubectl get svc

Check the version of nginx
---
http://{your public ip}/test

Create a green deployment pointing to latest version of nginx
---

vi green.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: greendeployment
  labels:
    app: web
spec:
  replicas: 4
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
        version: green
    spec:
      containers:
      - name: nginxpod
        image: nginx
        ports:
        - containerPort: 80

kubectl apply -f green.yaml

Modify the service.yaml to point to green deployment insted of blue deployment
---
vi service.yaml

apiVersion: v1
kind: Service
metadata:
  name: webservice
  labels:
    app: web
spec:
  ports:
  - port: 80
    protocol: TCP
  selector:
    app: web
    version: green
  type: LoadBalancer

kubectl apply -f service.yaml

refresh the webpage and Check the version of nginx
---
http://{your public ip}/test

Perform clean up
---
kubectl delete -f service.yaml
kubectl delete -f blue.yaml
kubectl delete -f green.yaml

Cronjob in kubernetes
---
vi cronjob.yaml

apiVersion: batch/v1
kind: CronJob
metadata:
  name: training
spec:
  schedule: "*/2 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: training
            image: bash
            command: ["echo", "Welcome to k8s training"]
          restartPolicy: OnFailure

kubectl apply -f cronjob.yaml

List the resources
----
kubectl get cj
kubectl get job
kubectl get po

to get the logs of a po
---
kubectl logs {your pod}

List the pods in wait mode
---
kubectl get po -w

delete cronjob directly
---
kubectl delete cj training

run a pod directly
----
kubectl run testpod -it --image bash -- bash

exit

run a command inside the teriminal
----
kubectl exec testpod -- ls
exit

take a shell session inside running pod
----
kubectl exec -it testpod -- bash

exit

Delete the pod
---
kubectl delete po testpod

run the pod with rm
----
kubectl run testpod -it --rm --image bash -- bash

exit

Stop your kubernetes cluster
---
azure -- kubernetes -- {your cluster} -- stop

Day 5
===

Start your kubernetes cluster
---
azure -- kubernetes -- {your cluster} -- start

List the nodes
---
kubectl get nodes

Navigate to kubernetes directrory
---
vi env.yaml

environment variable
----
vi env.yaml

apiVersion: v1
kind: Pod
metadata:
  name: environment
spec:
  containers:
  - name: envcontainer
    image: ubuntu
    command: ["sleep", "365d"]
    env:
    - name: TRAINING
      value: "devops"
    - name: TOPIC
      value: "k8s"
      
kubectl apply -f env.yaml

run env command inside pod to get the avalibale environment variables
---
kubectl get po
kubectl exec environment -- env

Create configmap with multiple variables
----
vi configmap.yaml

apiVersion: v1
kind: ConfigMap
metadata:
  name: trainingconfigmap
data:
  training: platform
  topic: k8s
  lab: azure
  keys: |
    name.training=platform
    lab.training=azure
    topic.training=k8s

kubectl apply -f configmap.yaml

create a pod to use env from configmap
---
vi configmappod.yaml

apiVersion: v1
kind: Pod
metadata:
  name: configmappod
spec:
  containers:
  - name: configmapcontainer
    image: ubuntu
    command: ["sleep", "365d"]
    envFrom:
    - configMapRef:
        name: trainingconfigmap
        
kubectl apply -f configmappod.yaml

run env command inside pod to get the avalibale environment variables
---
kubectl exec configmappod -- env

List the pod with node information
--
kubectl get po -o wide

Create a redis config file
----
vi redis-config

maxmemory 2mb
maxmemory-policy allkeys-lru

convert the redis configuration file into configmap
---
kubectl create configmap redis --from-file=redis-config

create a redis pod by mouting the redis config file from configmap
---- 
vi redis.yaml

apiVersion: v1
kind: Pod
metadata:
  name: redis
spec:
  containers:
  - name: redis
    image: redis:5.0.4
    command:
      - redis-server
      - "/redis-master/redis.conf"
    env:
    - name: MASTER
      value: "true"
    ports:
    - containerPort: 5963
    volumeMounts:
    - mountPath: /redis-master-data
      name: data
    - mountPath: /redis-master
      name: config
  volumes:
    - name: data
      emptyDir: {}
    - name: config
      configMap:
        name: redis
        items:
        - key: redis-config
          path: redis.conf


kubectl apply -f redis.yaml


Take a teriminal inside the pod and check the config file
---
kubectl exec -it redis -- bash
ls /redis-master/
cat /redis-master/redis.conf
exit

Delete the pods created
---
kubectl delete po configmappod environment redis

Create a yaml for multicontainer pod
----
vi multi.yaml

apiVersion: v1
kind: Pod
metadata:
  name: multi
spec:
  volumes:
  - name: data
    emptyDir: {}
  containers:
  - name: datagen
    image: debian
    command: ["/bin/sh","-c"]
    args:
      - while true; do
          date >> /html/index.html;
          sleep 10;
        done
    volumeMounts:
    - name: data
      mountPath: /html
  - name: datadis
    image: nginx
    volumeMounts:
    - name: data
      mountPath: /usr/share/nginx/html
      
kubectl apply -f multi.yaml

kubectl get po

get the log from specific containers
---
kubectl logs multi
kubectl logs multi -c datagen
kubectl logs multi -c datadis

to excute commnad on specific containers
----
kubectl exec multi -c datadis -- cat /usr/share/nginx/html/index.html
kubectl exec multi -c datagen -- cat /html/index.html

Delet the multicontainer pod
---
kubectl delete po multi

Create secret for mysql username and password
---
vi secret.yaml

apiVersion: v1
kind: Secret
metadata:
  name: mysql-credentials
  labels:
    "k8straining": lamp
type: Opaque
data:
  rootpw: dmFyTXlSb290UGFzcw==
  user: dmFyTXlEQlVzZXI=
  password: dmFyTXlEQlBhc3M=

kubectl apply -f secret.yaml

create db
---

vi db.yaml

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: database
  labels:
    "k8straining": lamp
spec:
  accessModes:
  - ReadWriteOnce
  storageClassName: default
  resources:
    requests:
      storage: 1Gi
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql
  labels:
    "k8straining": lamp
spec:
  replicas: 1
  revisionHistoryLimit: 5
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - image: mysql:5.7
        name: mysql
        resources:
          requests:
            cpu: "250m"
            memory: "256Mi"
          limits:
            cpu: 1
            memory: "1Gi"
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-credentials
              key: rootpw
        - name: MYSQL_USER
          valueFrom:
            secretKeyRef:
              name: mysql-credentials
              key: user
        - name: MYSQL_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-credentials
              key: password
        - name: MYSQL_DATABASE
          value: sakila
        livenessProbe:
          tcpSocket:
            port: 3306
        ports:
        - containerPort: 3306
        volumeMounts:
        - mountPath: /var/lib/mysql
          subPath: data
          name: database
      volumes:
      - name: database
        persistentVolumeClaim:
          claimName: database
---
apiVersion: v1
kind: Service
metadata:
  name: mysql
  labels:
    "k8straining": lamp
spec:
  type: ClusterIP
  ports:
  - port: 3306
    protocol: TCP
  selector:
    app: mysql


kubectl apply -f db.yaml

kubectl get pvc
kubectl get pv
kubectl get po
kubectl get svc

create a data loader job
---

vi dataloader.yaml

apiVersion: batch/v1
kind: Job
metadata:
  name: mysqldataloader
  labels:
    "k8straining": lamp
spec:
  activeDeadlineSeconds: 300
  template:
    metadata:
      name: mysqldataloader
    spec:
      containers:
      - name: mysqldataloader
        image: sathishbob/example-php-dbconnect
        env:
        - name: MYSQL_USER
          valueFrom:
            secretKeyRef:
              name: mysql-credentials
              key: user
        - name: MYSQL_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-credentials
              key: password
        - name: MYSQL_HOST
          value: mysql.default.svc.cluster.local
        command: ["/tmp/mysql-sakila-data-loader.sh"]
      restartPolicy: OnFailure

kubectl apply -f dataloader.yaml

kubectl get po

kubectl logs {your pod name}


deploy php app
----
vi phpapp.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: phpdbconnect
  labels:
    "k8straining": lamp
spec:
  replicas: 3
  revisionHistoryLimit: 5
  selector:
    matchLabels:
      app: phpdbconnect
  template:
    metadata:
      labels:
        app: phpdbconnect
    spec:
      containers:
      - image: sathishbob/example-php-dbconnect
        name: phpdbconnect
        imagePullPolicy: Always
        resources:
          requests:
            cpu: 100m
        env:
        - name: MYSQL_USER
          valueFrom:
            secretKeyRef:
              name: mysql-credentials
              key: user
        - name: MYSQL_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-credentials
              key: password
        - name: MYSQL_HOST
          value: mysql.default.svc.cluster.local
        livenessProbe:
          tcpSocket:
            port: 80
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: phpapp
  labels:
    "k8straining": lamp
spec:
  ports:
  - port: 80
    protocol: TCP
  selector:
    app: phpdbconnect
  type: LoadBalancer  

kubectl apply -f phpapp.yaml

kubectl get all

access the webapp
---
kubectl get svc
http://{your external ip}
http://{your external ip}/mysql-connect.php

do the clean up
---
kubectl delete -f phpapp.yaml
kubectl delete -f db.yaml
kubectl delete -f dataloader.yaml
kubectl delete -f secret.yaml

daemonset
----
vi daemonset.yaml

apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: flunentd
  labels:
    app: flunentd
spec:
  selector:
    matchLabels:
      name: flunentd
  template:
    metadata:
      labels:
        name: flunentd
    spec:
      containers:
      - name: flunentd
        image: registry.hub.docker.com/monotek/fluentd-elasticsearch:27
        volumeMounts:
        - name: varlog
          mountPath: /var/log
        - name: varlibdockercontainers
          mountPath: /var/lib/docker/containers
      volumes:
      - name: varlog
        hostPath:
          path: /var/log
      - name: varlibdockercontainers
        hostPath:
          path: /var/lib/docker/containers

kubectl apply -f daemonset.yaml

kubectl get po

kubectl get po -o wide


Scale the kubernetes node to 3
---
azure -- kubernetes services -- your cluster
settings -- node pool -- agent pool
scale node pool -- Minimum node count -- 3

Get the nodes and pods
---
kubectl get nodes

kubectl get po -o wide

Scale the kubernetes node to 2
---
azure -- kubernetes services -- your cluster
settings -- node pool -- agent pool
scale node pool -- Minimum node count -- 2

create configmap for sql config
---
vi ssconfigmap.yaml

apiVersion: v1
kind: ConfigMap
metadata:
  name: mysql
  labels:
    app: mysql
data:
  primary.cnf: |
    # Apply this config only on the primary.
    [mysqld]
    log-bin    
  replica.cnf: |
    # Apply this config only on replicas.
    [mysqld]
    super-read-only    

kubectl apply -f ssconfigmap.yaml

create services
----
vi ssservice.yaml


# Headless service for stable DNS entries of StatefulSet members.
apiVersion: v1
kind: Service
metadata:
  name: mysql
  labels:
    app: mysql
spec:
  ports:
  - name: mysql
    port: 3306
  clusterIP: None
  selector:
    app: mysql
---
# Client service for connecting to any MySQL instance for reads.
# For writes, you must instead connect to the primary: mysql-0.mysql.
apiVersion: v1
kind: Service
metadata:
  name: mysql-read
  labels:
    app: mysql
spec:
  ports:
  - name: mysql
    port: 3306
  selector:
    app: mysql

kubectl apply -f ssservice.yaml


create the startefull set
----

vi ss.yaml

apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
spec:
  selector:
    matchLabels:
      app: mysql
  serviceName: mysql
  replicas: 3
  template:
    metadata:
      labels:
        app: mysql
    spec:
      initContainers:
      - name: init-mysql
        image: mysql:5.7
        command:
        - bash
        - "-c"
        - |
          set -ex
          # Generate mysql server-id from pod ordinal index.
          [[ $HOSTNAME =~ -([0-9]+)$ ]] || exit 1
          ordinal=${BASH_REMATCH[1]}
          echo [mysqld] > /mnt/conf.d/server-id.cnf
          # Add an offset to avoid reserved server-id=0 value.
          echo server-id=$((100 + $ordinal)) >> /mnt/conf.d/server-id.cnf
          # Copy appropriate conf.d files from config-map to emptyDir.
          if [[ $ordinal -eq 0 ]]; then
            cp /mnt/config-map/primary.cnf /mnt/conf.d/
          else
            cp /mnt/config-map/replica.cnf /mnt/conf.d/
          fi          
        volumeMounts:
        - name: conf
          mountPath: /mnt/conf.d
        - name: config-map
          mountPath: /mnt/config-map
      - name: clone-mysql
        image: gcr.io/google-samples/xtrabackup:1.0
        command:
        - bash
        - "-c"
        - |
          set -ex
          # Skip the clone if data already exists.
          [[ -d /var/lib/mysql/mysql ]] && exit 0
          # Skip the clone on primary (ordinal index 0).
          [[ `hostname` =~ -([0-9]+)$ ]] || exit 1
          ordinal=${BASH_REMATCH[1]}
          [[ $ordinal -eq 0 ]] && exit 0
          # Clone data from previous peer.
          ncat --recv-only mysql-$(($ordinal-1)).mysql 3307 | xbstream -x -C /var/lib/mysql
          # Prepare the backup.
          xtrabackup --prepare --target-dir=/var/lib/mysql          
        volumeMounts:
        - name: data
          mountPath: /var/lib/mysql
          subPath: mysql
        - name: conf
          mountPath: /etc/mysql/conf.d
      containers:
      - name: mysql
        image: mysql:5.7
        env:
        - name: MYSQL_ALLOW_EMPTY_PASSWORD
          value: "1"
        ports:
        - name: mysql
          containerPort: 3306
        volumeMounts:
        - name: data
          mountPath: /var/lib/mysql
          subPath: mysql
        - name: conf
          mountPath: /etc/mysql/conf.d
        resources:
          requests:
            cpu: 200m
            memory: 500Mi
        livenessProbe:
          exec:
            command: ["mysqladmin", "ping"]
          initialDelaySeconds: 30
          periodSeconds: 10
          timeoutSeconds: 5
        readinessProbe:
          exec:
            # Check we can execute queries over TCP (skip-networking is off).
            command: ["mysql", "-h", "127.0.0.1", "-e", "SELECT 1"]
          initialDelaySeconds: 5
          periodSeconds: 2
          timeoutSeconds: 1
      - name: xtrabackup
        image: gcr.io/google-samples/xtrabackup:1.0
        ports:
        - name: xtrabackup
          containerPort: 3307
        command:
        - bash
        - "-c"
        - |
          set -ex
          cd /var/lib/mysql

          # Determine binlog position of cloned data, if any.
          if [[ -f xtrabackup_slave_info && "x$(<xtrabackup_slave_info)" != "x" ]]; then
            # XtraBackup already generated a partial "CHANGE MASTER TO" query
            # because we're cloning from an existing replica. (Need to remove the tailing semicolon!)
            cat xtrabackup_slave_info | sed -E 's/;$//g' > change_master_to.sql.in
            # Ignore xtrabackup_binlog_info in this case (it's useless).
            rm -f xtrabackup_slave_info xtrabackup_binlog_info
          elif [[ -f xtrabackup_binlog_info ]]; then
            # We're cloning directly from primary. Parse binlog position.
            [[ `cat xtrabackup_binlog_info` =~ ^(.*?)[[:space:]]+(.*?)$ ]] || exit 1
            rm -f xtrabackup_binlog_info xtrabackup_slave_info
            echo "CHANGE MASTER TO MASTER_LOG_FILE='${BASH_REMATCH[1]}',\
                  MASTER_LOG_POS=${BASH_REMATCH[2]}" > change_master_to.sql.in
          fi

          # Check if we need to complete a clone by starting replication.
          if [[ -f change_master_to.sql.in ]]; then
            echo "Waiting for mysqld to be ready (accepting connections)"
            until mysql -h 127.0.0.1 -e "SELECT 1"; do sleep 1; done

            echo "Initializing replication from clone position"
            mysql -h 127.0.0.1 \
                  -e "$(<change_master_to.sql.in), \
                          MASTER_HOST='mysql-0.mysql', \
                          MASTER_USER='root', \
                          MASTER_PASSWORD='', \
                          MASTER_CONNECT_RETRY=10; \
                        START SLAVE;" || exit 1
            # In case of container restart, attempt this at-most-once.
            mv change_master_to.sql.in change_master_to.sql.orig
          fi

          # Start a server to send backups when requested by peers.
          exec ncat --listen --keep-open --send-only --max-conns=1 3307 -c \
            "xtrabackup --backup --slave-info --stream=xbstream --host=127.0.0.1 --user=root"          
        volumeMounts:
        - name: data
          mountPath: /var/lib/mysql
          subPath: mysql
        - name: conf
          mountPath: /etc/mysql/conf.d
        resources:
          requests:
            cpu: 100m
            memory: 100Mi
      volumes:
      - name: conf
        emptyDir: {}
      - name: config-map
        configMap:
          name: mysql
  volumeClaimTemplates:
  - metadata:
      name: data
    spec:
      accessModes: ["ReadWriteOnce"]
      resources:
        requests:
          storage: 1Gi


kubectl apply -f ss.yaml

kubectl get po -w

CTRL + c

kubectl get po


run a pod with mysql image
----
kubectl run mysqlclient --image=mysql:5.7 -it --rm -- bash

connectt to mysql write master via headless service
---
mysql -h mysql-0.mysql

CREATE DATABASE test;
CREATE TABLE test.messages (message VARCHAR(100));
INSERT INTO test.messages VALUES ('hello');
SELECT * FROM test.messages;
exit

exit

runs a pod and try to execute a mysql query to read the data from mysql read service
---
kubectl run mysqlclient --image=mysql:5.7 -i --rm --restart=Never -- mysql -h mysql-read -e "SELECT * FROM test.messages"

run a pod with sql command to print the server id to the connection is made via read service
---
kubectl run mysqlclient --image=mysql:5.7 -i --rm --restart=Never -- bash -ic "while sleep 1; do mysql -h mysql-read -e 'SELECT @@server_id,NOW()';done"

take another teriminal and mimic a failure on one of the pod
---
sudo su
kubectl get po

kubectl delete po mysql-2

kubectl get po

Clean up the resources
---
CTRL + C

kubectl delete -f ss.yaml
kubectl delete -f ssservice.yaml
kubectl delete -f ssconfigmap.yaml
kubectl delete -f daemonset.yaml
kubectl delete po mysqlclient

install helm
---
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod +x get_helm.sh
./get_helm.sh

check the installation
---
helm version

create a chart
---
helm create training

show the structure of directory
---
apt install tree -y

tree training


Modify the values.yaml in
---
cd training

vi values.yaml

image:
  repository: nginx
  # This sets the pull policy for images.
  pullPolicy: IfNotPresent
  # Overrides the image tag whose default is the chart appVersion.
  tag: "1.26"


service:
  # This sets the service type more information can be found here: https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services-service-types
  type: LoadBalancer
  # This sets the ports more information can be found here: https://kubernetes.io/docs/concepts/services-networking/service/#field-spec-ports
  port: 80
  
dry run the installation
---
helm install webapp --dry-run --debug .

install the chart
---
helm install webapp .

list all the resources
---
kubectl get all

list the helm chart releases
---
helm list

modify image tag in values.yaml
---
vi values.yaml

tag: "latest"

upgrade the existing helm chart installation
---
helm upgrade webapp -f values.yaml .

delete a helm release
---
helm delete webapp

https://artifacthub.io/

install grafanachart
----
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
helm install dashboard grafana/grafana

helm upgrade dashboard grafana/grafana --set service.type=LoadBalancer

get the password
----
kubectl get secret --namespace default dashboard-grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo

download the helm chart to local
----
cd ..
helm pull grafana/grafana
tar -xvf grafana-8.11.1.tgz
cd grafana
ls -l

Post Assessment : https://forms.office.com/r/5ZSeQqwdPD

Feedback: https://forms.office.com/r/SKCf1ChQ0H

Email:
jegannathan.v@gmail.com
vikky2423@gmail.com
samridhi100@gmail.com

install istio
---
cd 
cd kubernetes

## download istio
curl -L https://istio.io/downloadIstio | sh -

cd istio-1.25.1/

export istioctl commnd location to path variable
----
export PATH="$PATH:/root/kubernetes/istio-1.24.1/bin"

do the precheck and install istio
----
istioctl x precheck
istioctl install --set profile=demo -y

check the installation
----
kubectl get ns
kubectl get all -n istio-system

create a namespace
----
kubectl create ns servicemesh

enable istio on the namespace
---
kubectl label namespace servicemesh istio-injection=enabled

inject istio on the deployment yaml (sample command no need to run it)
---
istioctl kube-inject -f your.yaml | kubectl apply -f -

install book info application
---
kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml -n servicemesh

create a gateway and virtual service
---
kubectl apply -f samples/bookinfo/networking/bookinfo-gateway.yaml -n servicemesh

list the services in the istio-system namespace
---
kubectl get svc -n istio-system

try to access the external ip of ingress
---
http://{your external ip}/

access the application
---
http://{your external ip}/productpage

create default routing rules
---
kubectl apply -f samples/bookinfo/networking/destination-rule-all.yaml -n servicemesh

create a virtual service for review with user based routing
----
kubectl apply -f samples/bookinfo/networking/virtual-service-reviews-test-v2.yaml -n servicemesh

check the url with out login
---
http://{your external ip}/productpage

check the usrl with login as jason user
---
http://{your external ip}/productpage

canary deploymente 50-50
---
kubectl apply -f samples/bookinfo/networking/virtual-service-reviews-50-v3.yaml -n servicemesh 

canary deploymente 100% to v3
---
kubectl apply -f samples/bookinfo/networking/virtual-service-reviews-v3.yaml -n servicemesh

Minikube instruction
---
https://minikube.sigs.k8s.io/docs/start/?arch=%2Fwindows%2Fx86-64%2Fstable%2F.exe+download


Fork the repo
---
https://github.com/sathishbob/javaapp-kuber

install docker pipeline
---
Dashboard  -- Manage Jenkins -- Plugins -- avaliable --  Docker Pipeline

configure docker tool
---
Dashboard  -- Manage Jenkins -- tools -- docker installations 
Add Docker
Name: docker
select "install automatically"
add installer "download from docker.com"
docker version : latest

Create credentails for dockerhub
----
Dashboard -- Manage Jenkins -- Credentials -- System -- Global credentials (unrestricted)
Kind : username with password
Username: {your dockerhub id}
Password: {your dockerhub passowrd}
ID: dockerhub
create

on putty add jenkins user to docker group
---
usermod -a -G docker jenkins

add jenkins users to sudoers file to run sudo command without password
---
visudo

## add the below line

# User privilege specification
root    ALL=(ALL:ALL) ALL
jenkins ALL= NOPASSWD: ALL

## to save 
Ctrl +x -- y -- enter

restart jenkins
----
systemctl restart jenkins

Modify jenkins file in your repo
---
pipeline {
    environment {
        imagename = "{your dockerhub id}/javaapp-jenkins-training"
        dockerImage = ''
        registryCredentials = 'dockerhub'
    }
    agent any
    tools {
        maven "MVN3"
        dockerTool "docker"
    }
    
    stages {
        stage("pullscm") {
            steps {
                git credentialsId: 'github', url: 'git@github.com:{your github repo id}/javaapp-kuber.git'
            }
        }
        stage("build") {
            steps {
                sh "mvn -f kubernetes-java clean install"
            }
        }
        stage("Build Docker Image") {
            steps {
                script {
                    dockerImage = docker.build("$imagename","kubernetes-java")
                }
            }
        }
        stage("push Docker image") {
            steps {
                script {
                    docker.withRegistry( '', registryCredentials ) {
                        dockerImage.push("$BUILD_NUMBER")
                        dockerImage.push('latest')
                    }
                }
            }
        }
        stage(Removeunusedimages) {
            steps {
                sh "docker rmi $imagename:$BUILD_NUMBER"
                sh "docker rmi $imagename"
            }
        }
        stage("kubedeployment") {
            steps {
                sh "sed -i s/latest/$BUILD_NUMBER/g kubernetes-java/deploy.yml"
                sh "sudo kubectl apply -f kubernetes-java/deploy.yml"
            }
        }
    }

}

modify the deploy.yaml under kubernetes-java
----

apiVersion: apps/v1
kind: Deployment
metadata:
  name: trainingjava
  labels:
    app: tomcat
  annotations:
    kubernetes.io/change-cause: Build no is test
spec:
  replicas: 1
  selector:
    matchLabels:
      app: tomcat
  template:
    metadata:
      labels:
        app: tomcat
    spec:
      containers:
      - name: tomcat
        image: {your dockerhub id}/javaapp-jenkins-training:latest
        imagePullPolicy: Always
        ports:
        - containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
  name: tomcatservice
spec:
  selector:
    app: tomcat
  ports:
  - protocol: TCP
    port: 8080
  type: LoadBalancer



create a new piple job
---
jenkins -- new item -- kubejavaapp -- pipeline

Build Triggers -- poll scm -- Schedule "* * * * *"

Definition: pipeline script from SCM
SCM: GIT
    repo URL: your repourl
    Credentials: git
    
save

build now

get the publicip form svc
---
kubectl get svc

access the application
---
http://{your public ip}:8080/newapp-0.0.1-SNAPSHOT/

in the git modify the file to mimic a git push
---
kubernetes-java/src/main/webapp/index.jsp

contact details
---
mail: sathishbob@gmail.com
whatsapp: 9884627727

Email:
prakaash005@gmail.com
vsnehamichelle@gmail.com
vishvarajsinhchauhan2601@gmail.com
samridhi100@gmail.com
