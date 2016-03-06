jenkins-dind
============

Outdated
-----
This repository is outdated. If you are looking for the newer project please follow here: [dind-builder](https://github.com/m1no/dind-builder). This project here will not be maintained anymore.

![Yo dawg, I herd you like Docker, so I put an Jenkins in your Docker so you can Docker while you Docker](https://i.chzbgr.com/maxW500/8402324480/h76778BF0/)

Requirements
-----
This docker container is intended to be used together with the [Jenkins Docker Plugin](https://wiki.jenkins-ci.org/display/JENKINS/Docker+Plugin).

Setup
-----
* Check that the required module, as stated above is installed
* Add a new credential in Jenkins with Username: jenkins and Password: jenkins
* Configure a new docker cloud in your Jenkins settings (Manage Jenkins->Configure System)
 * Give it a name and a valid docker URL like http://my.docker.host:2375
 * Test the connection
 * Add this image from the public registry to that cloud with the ID: m1no/jenkins-dind
 * Give that image a valid build label (ex.:"docker") to point your build jobs to it
 * Select the newly created credential from before to allow the Jenkins Master to connect
   via ssh to the new Docker Jenkins slave
 * Click the "Advanced..." button for that image
 * Enable "Run container privileged" mode
* Create a new build job and set the option "Restrict where this project can be run" to
  the new build label (ex.:"docker")
* Do your build steps as usual

After running a build you should see that Jenkins start a new docker container
everytime you trigger this job to build. Shortly after triggerin the Build, there will
be a notice that the job is pending on the build instance, this is totally normal. After
the brand new slave is fully operational this should go over in to "Building".

Usage
-----
* At our office we use it to build all our jobs in docker containers even
  other docker containers
* It helps a lot to have a deterministic docker build setup all the time

Known Issues
------------
* The Docker Plugin of Jenkins does not clean the attached container volumes
  => Manual cleanup needed on the Docker main host regularly
  ```rm -Rf /var/lib/docker/vfs/dir/*```
