## Training Note1

Short url for codeshare: https://shorturl.at/Nk8IY

#### Day1
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
