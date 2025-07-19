# How to run Ansible playbook from Jenkins pipeline job

https://www.geeksforgeeks.org/devops/using-ansible-in-jenkins-pipelines/
https://medium.com/@mudasirhaji/how-to-configure-nginx-as-a-reverse-proxy-on-aws-ec2-instance-270736ca2a50

Step 1: Launch EC2 Instance
First go to AWS Console and login by using your credentials or create new account
Now go to EC2 dashboard and Launch instance
Now connect with terminal


Step 2: Install Ansible and Jenkins
Install ansible and Jenkins in our local machine by using following commands

To install Jenkins packages follow below commands

      sudo wget -O /etc/yum.repos.d/jenkins.repo \https://pkg.jenkins.io/redhat-stable/jenkins.repo
      sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key 

  Install java because jenkins run times is java so we need to install java :
      sudo yum install java-17-openjdk -y
      sudo yum -y install java-17*

  Now install jenkins by using following command,
      sudo yum -y install jenkins
      
  After completion of jenkins installation, now start and enable the jenkins and also check status of the jenkins by using following commands
      sudo systemctl start Jenkins
      sudo systemctl enable Jenkins
      sudo systemctl status Jenkins

  Now copy Public IP of your instance and Browse it along with Port 8080, because jenkins runs on port 8080.

  Unlock jenkins by using administration password, administration password was shown in while check in status of the jenkins or either we can see the password in below path
    sudo cat /var/lib/jenkins/secrets/initialPassword

Official page of jenkins


Now install Ansible by using following command
  sudo dnf install ansible -y



Step-3: Install Ansible Plugin
Go to jenkins Dashboard --> Manage Jenkins --> Available Plugins --> Ansible


Step 4: Configure Ansible in Jenkins
Now configure ansible in jenkins
Go to Jenkins dashboard --> Manage Jenkins --> Tool Configuration
Now add ansible installation by providing path to ansible executable: /usr/bin


Step 5: Add Credentials
For ansible credentials are required for this
Go to manage Jenkins --> Credentials --> Add credentials
add key pem of the instance


Step 6: Create Playbook
Now create playbook by using YAML file


Step 7: Define Jenkins Pipeline
Now define jenkins pipeline
Go to jenkins dashboard and click on New item and choose pipeline


Give description to your project in pipeline script and write pipeline script
pipeline {
    agent any
    
    stages {
        stage("SCM checkout") {
            steps {
                git credentialsId: 'marleneRaissa-git-credentials', url: 'https://github.com/marleneRaissa/Blog.git'
            }
        }
        
        stage("Execute Ansible") {
            steps {
                ansiblePlaybook credentialsId: 'private-key',
                                 disableHostKeyChecking: true,
                                 installation: 'Ansible',
                                 inventory: 'dev.inv',
                                 playbook: 'apache.yml'
            }    
        }    
    }
}

Now run the pipeline


Step 8: Verify
Now go to EC2 dashboard copy Public IP of slave node and browse it.
