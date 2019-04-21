#### Basic Requirements To Run Jenkins

1. Run the following command to update your operating system before moving on to other packages installation

           $ apt-get update

2. Basic Web Server

   Jenkins requires the basic web server installed on your system, so if you don’t have any of your web server installed on your 
   operating system, you can execute the following command to install the web server.

          $ apt-get install nginx

 Once your Nginx web server is installed check the status of its services with the command below.

          $ service nginx status

3. Java Installation

   Install Java 8 from the Oracle website.

          $ sudo add-apt-repository ppa:webupd8team/java
          $ sudo apt-get update
          $ sudo apt-get install oracle-java8-installer

   To check status run:

          $ java –version


##### Installing Jenkins on Ubuntu

1. Now we are ready to install `Jenkins`. So before the package installation, we have to add the key and source list to apt for 
   Jenkins. To do so, issue the following two commands in the terminal

         $ wget -q -O - https://pkg.jenkins.io/debian/jenkins-ci.org.key | sudo apt-key add -

  You will get the `OK` status after you add the key. For the source list, here is the command to run:

         $ sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'

2. Now it needs to update `apt’s` cache before moving to Jenkins so that it can refresh the operating system’s repository for 
   the latest packages

        $ sudo apt-get update

3. Once your system is updated, execute the command below for the installation of Jenkins and type the `Y` key to proceed with 
   the installation process.

        $ sudo apt-get install jenkins

#### Upgrade

## Once installed like this, you can update to the later version of Jenkins (when it comes out) by running the following commands:

        $ sudo apt-get update

        $ sudo apt-get install jenkins

#### To check the status of Jenkins service, the command below will show you its status with running process ID.

        $ service jenkins status

#### Jenkins runs on port 8080 by default for HTTP connector. Now let’s open your web browser to use its GUI.

#### Jenkins GUI Configurations

1. When open jenkins port 8080 we will get one popup Window indicating to 'Unlock Jenkins'. To unlock jenkins we have to provide 
   Adminstrator Password in Input Box. we will get administration Password by below command:

        $ sudo cat /var/lib/jenkins/secrets/initialAdminPassword

2. Once u done with step 1 next screen we will get to 'Custamize Jenkins' contain plugins extend Jenkins with additional features
   to support many different needs.Here we have two select boxes.

   a. Install Suggested Plugins - ## contains plugins the Jenkins Community finds most useful

   b. Select Plugins To Install - ##  select and install plugins most suitable for your needs

3. next step is either to create first Admin User or continue with admin.if u want to create new user then enter the required 
   details and click on save and finish button.

#### above are the basic configuration of jenkins..after this we can see the welcome screen of jenkins where you will be able to configure its different parameters for your system..

#### Click on `Manage Jenkins` and then choose the required parameter to start required configurations.

     1. Configure System - To configure the global settings and paths for the Jenkins configurations.

     2. configure global security - secure jenkins, who is allowed to access/use the system.

     3. Manage Plugins - To install plugins in Jenkins, select this and search for the plugin you want to install  Select it
       from the list and click to download and install button.

##### Scheduling new Jobs with Jenkins

1. To schedule your new jobs, click the `New Item` on the top left of your Jenkins Dashboard, then enter a name for the job and 
   select the required job type.for example `Freestyle Project`. Press OK to create a new Job.

2. The next page allows you to configure your job.this page contains six coloumns:
   1. General: it contains various options like giving name and description to project and another important one is 
      Shared Workspace.each jenkins job contains workspace this can be shared with the Shared workspace plugin.we need to
      download and install from mange plugins in 'Manage Jenkins'.after that go to 'configure System' and add workspace in 
      'Sharing workspace' fields.

   2. Source Code Mangement: if u want to get code from any git repository,we first install git plugin as mentioned above.it is 
      installed with difault installations provided by jenkins community.there we add the location of repository and get the
      source code.

   3. Build Triggers: used to run the jobs according to our requirement by selecting it checkboxes

   4. Build Environment: is to build ur job on your required environment

   5. Build: this is an important section.expand the `Add Build Step` option and select the required to to execute ur job.if we 
      select `Execute Shell` where you will put the command that you wish to execute in the terminal.

   6. Post Build Actions: expand the add post-build actions`Add post-build action` and select the required checkbox.it is used 
      send notifications after build action completed.

3. save the configuration for the job after added according to u r requirement.

4. Once the next page is done loading, click the `Build Now` button on the sidebar of your dashboard.

5. from above u will get `Build History` down in the same page, we can check output of our job on clicking the job.blue ball 
   indicates success of our job, if it fails ball is in red.

6. job fails for sudo commands given in `Execute Shell` in Build.it is due to we have to give sudo permissions for jenkins users.
   we have to edit sudoers file in our local machine
   
        $ sudo su

        $ visudo -f /etc/sudoers

   add following line at the end in sudoers file.

   jenkins ALL= NOPASSWD: ALL





 
