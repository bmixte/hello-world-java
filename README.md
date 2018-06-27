# We will explain how to make an automatic build and deployment directly from source code to application platform.
# Just follow this step by step if you want to try
# 
# connect to github and create a new repository. (in the next the repository name is "hello-world-java")

# Initializing git new repo from your code directory
# Go to your source code directory
# You can start from clone my repository if you want.
cd /anEmptyDirectory/
git clone https://github.com/bmixte/hello-world-java.git
# initalize the directory to push it in your github own repository
rm -rf .git 
git init
# Change the repository URL.
git remote add origin https://github.com/myOwnRepo
# refresh repository with your last code
git add --all
git commit -m "initial commit"
git push origin master

# Login to openshift.
# on web browser : https://manawa.euw1-gcp-poc.adeo.cloud/
# on command line (off course change login)
oc login manawa.euw1-gcp-poc.adeo.cloud -u 20007790

# create a new openshift project in command line
oc new-project devweek-bmixte --description="New project for devweek demo" --display-name="just a java hello-world application"
set the context/namespace
oc project devweek-bmixte

# create a new openshift application from github source code. (S2I, source to image)
oc new-app registry.access.redhat.com/redhat-openjdk-18/openjdk18-openshift~https://github.com/bmixte/hello-world-java.git --name devweek-bmixte -e JAVA_APP_JAR=hello-world-0.2.0.jar

# add route to access to application.
add route by interfaces …

# add webhook on github to follow code change …
# from openshift get webhook URL.
oc describe bc/devweek-bmixte

# modify webhook json to create github webhook in command line
vim webhookapi.json
curl -X POST --data @webhookapi.json -u "bmixte" https://api.github.com/repos/bmixte/hello-world-java/hooks 

# Make change on code and follow what's happened on openshift.
# add picture to directory
# modify index.html to use the new picture

# push to or update git origin
git add --all
git commit -m "change picture"
git push -u origin master

# Follow automatic deployment on openshift
# It's all ... Your code can automatically and without hand be deployed directly on Application platform.
# Thanks to follow me.