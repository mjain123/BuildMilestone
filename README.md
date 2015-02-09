# BuildMilestone

### Introduction
We have used Jenkins Application and have configured it to work according our requirements.
Jenkins is a easy to use continuous integration application which helps developers to integrate changes to the project, and makes it easier for users to obtain a fresh build. It provides various features which can be read [here].

### Build
Jetkins was installed on a windows machine using the windows package available at its [website].

The default home directory of Jetkins is - C://Program Files (x86)//Jenkins.
The configuration setup was done using the dashboard by runnning it at localhost. By default it works at port 8080. 
http://localhost:8080/
We need to provide path to the git executable by - Click Manage Jenkins -> Configure System. [image]

Next we can start creating a new job for Jenkins. Click on new job. Select Freestyle project and give name. Click next. 
Here we need to provide link to Github Project and then Github repository. Credentials to access can also be provided here.
Then we selected to trigger build periodically and passed the value - H * * * *. This builds the project every hour on every day.
After saving the setting, we can go back to job project and click on Build Now to initiate building. 
By clicking on the build, we can check the console output.


To enable jenkins to trigger a build the project in response to a git commit, we modified the git hook file present in the .git directory. We made the following changes in the post-commit file - Add line
```sh
/usr/bin/curl http://localhost:8080/job/demo4/build?token=401ca1bd52edc632f8e6ab17563db9a42291ac3b
```
where the token value was generated from github.com and the initial part of the address is the url of the job on jenkins.


Jenkins provide various remote access APIs [link]. We have used the json api to access the last build status via http.
The following http link provides the status of the last build in JSON format.
```sh
http://localhost:8080/job/demo3/lastSuccessfulBuild/api/json
```

[here]:https://wiki.jenkins-ci.org/display/JENKINS/Meet+Jenkins
[website]: https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins#InstallingJenkins-WindowsInstallation
[link]:  https://wiki.jenkins-ci.org/display/JENKINS/Remote+access+API
