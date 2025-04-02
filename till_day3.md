Welcome to Adv DevOps Training
====
Short url for codeshare: https://shorturl.at/Nk8IY
Lab URL:	https://virtuallabs.palmeto.co.in/	

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
