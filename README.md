# ansible-aws-cmt
Objective: Launch a Node.js application in a Docker container and connect it to My SQL Database using environment variables

provision RDS database and EC2 instance, then set up a connection between RDS Database and EC2 instance with ansible YAML source:
Extend the playbook to deploy a web application that uses the RDS database. Launch a web application in in a Docker container using public image.

A)Prerequisites:
---------------
Before running the playbook, install the required Ansible collections on control Node:

A) SSH KEY SETUP and connect to Ansible control Node
scp -i "ecr-key.pem" ecr-key.pem ec2-user@PUBLIC-IP-ADDRESS-OF-CONTROL-NODE:/home/ec2-user/
ssh -i "ecr-key.pem" ec2-user@PUBLIC-IP-ADDRESS-OF-CONTROL-NODE

B)install ansible on control Node
1 sudo yum update -y
2 sudo yum install ansible -y
3 ansible --version
4 ansible-galaxy collection install amazon.aws community.docker
5 sudo yum install awscli
6 sudo yum install python3-pip -y
7 pip install boto3 botocore  #Running pip as the 'root' user can result in broken permissions and conflicting behavior with the system package manager.

C).SSH AGENT SETUP on on control Node
sudo systemctl status sshd
eval $(ssh-agent -s)
ssh-add ecr-key.pem  #run this command in home directory: /home/ec2-user
ssh-add -l
ssh ec2-user@PRIVATE-IP-ADDRESS-OF-NODE2

D) run AWS configuration
aws configure

E)How to Run playbook:
# 1.Set your RDS password securely via environment variable
export RDS_PASSWORD="yourPASSPHRASE32#"

# 2.Run the playbook
ansible-playbook site.yml
  
F) Connect to RDS DB using mysqlclient (on Ec2 managed server on Linux/unix O/S)
------------------------------------------
1.sudo dnf install mariadb105
2.curl -o global-bundle.pem https://truststore.pki.rds.amazonaws.com/global/global-bundle.pem
3.mysql -h webapp-mysql-db.caj6cqmi6xl7.us-east-1.rds.amazonaws.com -P 3306 -u admin -p --ssl-ca=./global-bundle.pem

