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
How to use:
	URL:
	Logging in:
	Manualy running the job
	storage path in .env

Maintainace:
	Log retention:
	Upgarade Jenkins versions:

Design:

The setup is to run on a windows 10 desktop using docker desktop and behind NAT making the setup not accessable by webhooks on services such as github ot gitlab.
After some research - I opted to use github API and not a Jenkins plugin.
The jenkins job will run and will make a rest call to the githum API, obtaining a JSON of the currect PR/merege requets.
If any PR's are in open state - will run a python script printing out "Devops is great".
(In the real world we will move the PR state to pending and run tests to approve/warn/deny the PR.)


General notes:

SSL/TLS was not implemened due to the server running on localhost.
The BKC is to limit using groovy opbject such as JsonSlurper / HttpRequest (https://www.jenkins.io/doc/book/pipeline/pipeline-best-practices/)
this lead to the usege of curl and a basic parsing script.