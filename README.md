# MyJenkins
Hello world from a Jenkins upon pull/merge request

This repo will enable to run a docker based jenkins server that monitors a git repository for merge requests.
Upon such a request a Jenkins job to trigger and will:
	1. search for new PR's/.
	2. Print "Devops is great".
	3. Print the merge request name(s).

Requirments:
docker insgtalled. 
docker-compose add on installed.


How to install/run:

I created 2 options:
1. Ubtain the Jenkins.zip from this link https://drive.google.com/file/d/1e8uDQ8yiEdowipoxJrhZP5PyKvByeRSt/view?usp=sharing, this is a copy of the configured Jenkins server:
	1.1 Unzip the zip file to a folder (say c:\storage)
	1.2 in the repo folder - update the file .env with the path of the new folder (say "STORAGE=/c/storage/")
	1.3 in the repo folder - start the jenkins docker (see below)
	1.4 all the content/configuration is ready to use.
	1.5 the job to use is named ValidateNewPullReqests
	
2. Configure jenkins from start:
	2.1 Create a folder c:\storage
	2.3 Create a folder c:\storage\jenkins_home
	2.4 Create a folder c:\storage\var
	2.5 Create a folder c:\storage\var\run
	2.6 Create a folder c:\storage\var\run\docker.sock
	2.7 in the repo folder - update the file .env with the path of the new folder (say "STORAGE=/c/storage/")
	2.8 in the repo folder - start the jenkins docker
	2.9 run the command "docker-compose up -d"
	2.10 wait a bit :-) 
	2.11 Access jenkins at http://localhost:8080
	2.12 unlock the server ( password is in jenkins_home/secrets/initialAdminPassword)
	2.13 Configure ( I will not go into this process here) 
	

Starting Jenkins:
1. change the repository folder you cloned.
2. run the command "docker-compose up -d"
3. wait a bit :-) 
4. Access jenkins at http://localhost:8080
5. Configured username/password: admin/Admin!98

Running the job:
1. Validate the you have a gitlab connection named "GitHub Credentials" with ID "kokoisN1"


Design:

The setup is to run on a windows 10 desktop using docker desktop and behind NAT making the setup not accessable by webhooks on services such as github ot gitlab.

After some research - I opted to use github API and not a Jenkins plugin.
(The github and gitlab pull request plugins are classic, they have no working DSL syntax...)

The jenkins job will run and will make a rest call to the githum API, obtaining a JSON of the currect PR/merege requets.
If any PR's are in open state - will run a python script printing out "Devops is great".
(In the real world we will move the PR state to pending and run tests to approve/warn/deny the PR.)

Setup design:
The setup is based in windows 10, using docker desktop / WSL 2 / docker-compose.
The Jenkins docker is created using the provided Dockerfile, by runing "docker build ." basded of jenkins/jenkins:2.361.2-jdk11 release.
Python will be run of release python:3, the internal Jenkins continer will connect/trigger the cuntainer by the host setup via tcp to port 2376 on the host.

General notes:

SSL/TLS was not implemened due to the server running on localhost.
The BKC is to limit using groovy opbject such as JsonSlurper / HttpRequest (https://www.jenkins.io/doc/book/pipeline/pipeline-best-practices/)
this lead to the usege of curl and a basic parsing script.