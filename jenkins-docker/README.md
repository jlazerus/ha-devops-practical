DevOps Practical Exercise - Jenkins Instance
============================================

This folder contains the necessary elements to get a functional Jenkins server off the ground with some basic plugins installed

Building the Instance
---------------------

The [Dockerfile](Dockerfile) can be built as such:

```
docker build -t ha-devops-practical-jenkins .
```

The Dockerfile does the following:
* Inherits from the latest `jenkins/jenkins:lts-alpine` image
* Copies the [plugins.txt](plugins.txt) into the correct location for automatic installation
* Installs all "Suggested" plugins and their dependencies from the Setup Wizard into the instance for use on initial boot

Running the Instance
--------------------

The following ports are automatically exposed in the inherited image:
* 8080
* 5000
 
If you are only going to run a single instance, and there should not be a need to run multiple, you can ignore port conflicts and map that port from your host into the container directly. If you wish to run multiple instances or choose not to map those ports directly to accomodate other services running on your machine, make note of the random port Docker assigns your instance in order to use the Jenkins instance.

You can run the instance with a command similar to:

```
docker run --name ha-devops-practical-jenkins -d -p 8080:8080 -p 5000:5000 ha-devops-practical-jenkins
```

This image does not skip the Setup Wizard, in case you want to select any additional plugins to install, add any users, etc. You will need to collect the initial admin password from `/var/jenkins_home/secrets/initialAdminPassword` inside the container after startup in order to complete the Setup Wizard.

Configuring the Instance
------------------------

Once your instance is up and running, and you have installed any additional plugins you might wish, you should be ready to configure a job to monitor the branch you created in this Git repository.