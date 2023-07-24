# code-deploy-and-code-pipeline
I had done this project using code deploy and code pipeline
Setup in Brief:
I have used two EC2 instance of AMZ2 Linux. First one is the web server we will be configuring, also called CodeDeploy agent. Second EC2 machine is supposed to use by developer where the codes are programmed. The names of the resources in the experiment are arbitrary and may name the resources your own.
1.Create IAM Roles for EC2-S3-CodeDeploy access.
2.Create IAM user account for developer
3.Install and prepare the CodeDeploy agent on webserver.
4.Create the code from Developer machine
5.Create Codedeploy Application and Push the code to S3 bucket from Developer machine
6.Create Deployment Group to include web server
7.Create Deployment to push the code to the Webserver
8. Test the website configuration


Use comands as per the need.
------------------------------
# wget https://aws-codedeploy-us-east-1.s3.amazonaws.com/latest/install
# chmod +x install
#./install auto
# service codedeploy-agent status
yum install ruby -y
yum update

Sample HTML code.
-----------------
<html>
<h2> Sample App Version 1 </h2>
</html>

Create appspec.yml
------------------
version: 0.0
os: linux
files:
 - source: /index.html
   destination: /var/www/html/
hooks:
 BeforeInstall:
  - location: scripts/httpd_install.sh
    timeout: 300
    runas: root
  - location: scripts/httpd_start.sh
    timeout: 300
    runas: root
 ApplicationStop:
  - location: scripts/httpd_stop.sh
    timeout: 300
    runas: root

----------***__---------**___-------

mkdir scripts
cd scripts
vi httpd_install.sh
vi httpd_start.sh
vi httpd_stop.sh

----------------------------------

#!/bin/bash
yum install -y httpd

#!/bin/bash
systemctl start httpd
systemctl enable httpd

#!/bin/bash
systemctl stop httpd
systemctl disable httpd
-----------------------------------

# aws deploy create-application --application-name sampleapp
-----------------------------------
# aws deploy push --application-name sampleapp --s3-location s3://aws24092021/sampleapp.zip
-----------------------------------
# zip -r ../sampleapp.zip .
-----------------------------------
# aws s3 cp sampleapp.zip s3://aws280921
-----------------------------------

Try to get SNS mail as sample below:

SNS NOTIFICATION:

{"account":"25694782316","detailType":"CodeDeploy Deployment State-change Notification","region":"ap-south-1","source":"aws.codedeploy","time":"2021-09-28T07:36:25Z","notificationRuleArn":"arn:aws:codestar-notifications:ap-south-1:984260169519:notificationrule/5c41dade30aebf84b52f5fa903e55a2d159eca4f","detail":{"application":"sampleapp","deploymentId":"d-F78DODA9C","deploymentGroup":"mydeploymentgroup","instanceGroupId":"96f08408-9ca3-4010-904e-286bed6c1780","state":"SUCCESS","region":"ap-south-1"},"resources":["arn:aws:codedeploy:ap-south-1:984260169519:deploymentgroup:sampleapp/mydeploymentgroup","
arn:aws:codedeploy:ap-south-1:984260169519:application:sampleapp"],"additionalAttributes":{}} 
------------------------------------
Note:
DEMOLISH:
1.DELETE INSTANCES
2.DELETE CODE DEPLOY
3.DELETE CODE PIPELINE
4.DELETE BUCKET AND OBJECT
5.DELETE SNS TOPIC AND SUBSCRIPTIONS
6.DELETE IAM ROLES AND USERS
---------------------------------------
