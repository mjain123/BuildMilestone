# BuildMilestone

### Introduction
We have used Jenkins Application and have configured it to work according our requirements.
Jenkins is a easy-to-use continuous integration application which helps developers to integrate changes to the project, and makes it easier for users to obtain a fresh build. It provides various features which can be read [here].

### Build
Jenkins was installed on a linux machine using the jenkins.war file available int its [website].

The default home directory of Jetkins is - /home/sujith/.jenkins.

The configuration setup was done using the dashboard by runnning it at localhost. By default it works at port 8080. 
http://localhost:8080/

There were certain plugins that are required to make Jenkins environment suitable to our build server.

We installed various plugins for -
Git
EC2
JAVA
SSH
Windows/Ubuntu Slaves
Maven
Credentials
S3
PAM

First we configured Jenkins on the localhost - On the dashboard Click Manage Jenkins -> Configure System

1 We added jdk and maven configurations to Jenkins.
2 We then did Git installations to connect it to github.
3 We provided Global configurations for Github to access our repo.
[image1]


Next we can start creating a new job for Jenkins. Click on new job. Select Freestyle project and give name. Click next. 
Here we need to provide link to Github Project and then Github repository. Credentials to access can also be provided here. We have cloned  [SkymindIO/nd4j] for this milestone. Jenkins provides an opportunity to choose the required branches that are to be built. We chose 'master'.

Then we selected to trigger build periodically and passed the value - H * * * *. This builds the project every hour on every day.

After saving the settings, we can go back to job project and click on Build Now to initiate the build. 
By clicking on the build, we can check the console output.

##Build Server properties

1. The ability to trigger a build in response to a git commit via a git hook.

To enable jenkins to trigger a build the project in response to a git commit, we modified the git hook file present in the .git directory. We made the following changes in the post-commit file - Add line
```sh
/usr/bin/curl http://localhost:8080/job/demo4/build?token=401ca1bd52edc632f8e6ab17563db9a42291ac3b
```
where the token value was generated from github.com and the initial part of the address is the url of the job on jenkins.
[image2]

2. The ability to setup dependencies for the project and restore to a clean state.

For our project, the server built installed all the necessary dependencies and to restore to a clean state we did - mvn clean install
[image3]
[image4]

3. The ability to execute a build script (e.g., shell, maven)

In the build configurations of the project, we added Maven version 3.2.1 and also provided it with a Goal "clean install" thus the Maven executes pom.xml files in the given repo, every first time a new repo is built on our server

[image5]
[image6]

4. The ability to run a build on multiple nodes (e.g. jenkins slaves, go agents, or a spawned droplet/AWS.).

We had created instances on EC2 and used them as slaves in our project. 

Using the EC2 plugin, we provided credentials to Amazon Web Services (Our AccessKey ID and Secret Access Key).

Then for the instances we had on EC2, we created AMI for the same on Jenkins. We also added a basic script to install JAVA on the instance to match our project requirements. 

In the Jenkins Dashboard, Click Manage Jenkins-> Manage Nodes. Here using the EC2 instances, we created new nodes, which act as slaves for this job. These slaves inherit all the configurations made on AMI in EC2 plugin. Thus when there are new builds, more than what master can accomodate, they are re-directed to these slaves and are executed on them.

[image7]
[image8]
[image9]

5. The ability to retrieve the status of the build via http.

Jenkins provide various remote access APIs [link]. We have used the json api to access the last build status via http.
The following http link provides the status of the last build in JSON format.
```sh
http://localhost:8080/job/nd/lastSuccessfulBuild/api/json
```
[image10]

[here]:https://wiki.jenkins-ci.org/display/JENKINS/Meet+Jenkins
[website]: https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins#InstallingJenkins-WindowsInstallation
[link]:  https://wiki.jenkins-ci.org/display/JENKINS/Remote+access+API
