# Introduction to Declarative Pipelines - Environment

This document describes how to create a shared CloudBees Jenkins Enterprise Master environment on AWS that the participants will use.

**Note**: You will need an AWS account and knowledge of how to create EC2 instances (https://aws.amazon.com/documentation/ec2/).

## Recommended EC2 Instance Types

The following is a rough guildeline to the AWS instance types that work well for for the workshop based on the number of users participating (giving each participant at least one dedicated executor as participants will frequently be building jobs at the same time):

| Users | Executors | Instance Type   | vCPU     | Memory     |  Cost Per Hour |  Cost Per Day |
|-------|-----------|-----------------|----------|------------|----------------|---------------|
| <= 4  | 4         | **t2.large**	  | 2	     | 8	      | $0.094         | $2.256        |
| <= 9  | 8         | **t2.xlarge**	  | 4	     | 16	      | $0.188         | $4.512        |
| <- 16 | 16        | **t2.2xlarge**  | 8	     | 32	      | $0.376         | $8.648        |

**Notes**:
  - Recommendation based on creating 2 executors per vCPU
  - Additional validation needs to be performed

# Create the AWS Instance

The following instructions can be used to create a new EC2 instance to host the CJE Master for the workshop:

1. Navigate to the EC2 console (https://console.aws.amazon.com/ec2/) and login;
2. Click on **Instances** in the **Instances** Menu;
3. Click on **Launch Instance**;
4. Click **Select** for **Amazon Linux AMI 2017.03.1 (HVM), SSD Volume Type**;
5. Select the proper **Instance Type** for your workshop requirements (see **Recommended EC2 Instace Types** above);
6. Click **Review and Launch**;
7. Click **Edit Security Groups**;
8. Modify your **Security Group** settings to ensure that the required Jenkins ports are open for SSH and HTTP;
9. Click **Review and Launch**;
10. Make sure the settings are correct and then click on **Launch**.
11. Select the key pair that you will use to connect to the instance and complete the creation process.

Once your instance is up and running you will need to connect to it via SSH to complete the installation process (http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html).

# Install the CloudBees Jenkins Enterprise Master

Prior to installing the CJE Master on your EC2 instance you need to update the instance and install Docker and Git.

## Update the Instance

First update the instance using:

```sudo yum update -y```

## Install Docker

Install docker using the following commands:

```
sudo yum install -y docker
sudo service docker start
sudo usermod -a -G docker ec2-user
```

For the user change made in the last line to take place you need to log out and back in to the instance:

```logout```

## Install Docker Compose


The following commands download version 1.6.1 of docker-compose (the latest when this project was last updated), set ownership on the executable, move it to the /usr/local/bin directory, and modify its execution permissions.

```
curl -L https://github.com/docker/compose/releases/download/1.6.1/docker-compose-`uname -s`-`uname -m` > docker-compose 
sudo chown root docker-compose
sudo mv docker-compose /usr/local/bin
sudo chmod +x /usr/local/bin/docker-compose
```

## Install Git and Clone Cje-master-install Repository

The easiest way copy the project files to the EC2 instance is to use Git to clone the files. Git isn't installed on the instance by default so you will need to install it using:

```sudo yum install -y git```

Once Git is installed you can clone this project using the following command from the ec2-user home directory (/home/ec2-user):

```sudo git clone https://github.com/PipelineHandsOn/cje-master-install```

This will clone the project files into:

```/home/ec2-user/cje-master-install```


## Build the CJE Master Docker Image

Before you run Jenkins for the first time you need to build the Docker image it will user. The first step navigate to the project folder using:

```cd cje-master-install```

Then run the following Docker commands:

```
docker pull cloudbees/jenkins-enterprise
docker-compose build
```

Once the build process completes you are ready to start your Jenkins instance.

## Start the CJE Master

To start Jenkins use the following command from within the ```cje-master-install``` directory:

```docker-compose up -d```

Note: If the is the first time you are running Jenkins you will need to get the instance's initial administrative password from secrets. Your JENKINS_HOME directory will be ```/home/ec2-user/jenkins``` and you can use the following command to output the password:

```sudo cat ~/jenkins/secrets/initialAdminPassword```

Your Jenkins instance will be available on port ```8080``` unless you modify the projects ```docker-compose.yml``` file.


# Configure the Master

Once the master has started you will need to log in and configure it to make the master useful for the workshop. The configuration includes:

  * Completing the installation (add the license key, install required plugins, change admin user password)
  * Setting up the environment (create folders for each user participating in the workshop)
  * Verifying that the master is configured correctly by running some of the examples prior to the start of the workshop

## Configure the Master

The first time you access the new master you will need the ```initialAdminPassword``` copied in the **Start the CJE Master** section above to log in. You will also need a valid license key to complete the configuration process.

**Note**: For the purpose of this document we will assume you already have a license key.

1. Log in!

2. Select **Use a License Key**, copy and paste your license key into the **Enter you license key** form, select **I accept the evaluation or standard license**, and click on **Submit**.

3. Click on the **Install Select Plugins** option in the **Customize Jenkins** form.

4. When your plugins are installed select **Continue as Admin** and then **Start Using CloudBees Enterprise Jenkins**.

**Important**: The first step you should perform once you are logged in is to change the Administrator password to something secure that you won't forget!


## Setup the Workshop Environment

The following section covers the steps that should be taken to configure the master for the workshop.

### Add Executors

By default masters only have 2 executors. We need to add additional executors to support multiple students running concurrent jobs on the master.

1. Click on the **Manage Jenkins** link
2. Click on **Configure System**
3. Update the value in the **# of executors** field to match the number chosen for your master (see instance guidance above)
4. Click on **Save**

### Update Plugins

Depending on when the workshop is given the material in the workshop (and the plugins used) may be ahead of the versions of plugins in the CAP program. To update the master to the latest version of pipeline we are going to disable **Beekeeper** and update our instance to the latest plugins.

1. Click on the **Manage Jenkins** link
2. Click on **Beekeeper Upgrade Assistant**
3. De-select **Enroll this instance in the CloudBees Assurance Program** and click **Save**
4. Click on the **Manage Jenkins** link
5. Click on the **Manage Plugins** link
6. Scroll to the bottom of the **Updates** tab and click on the **All** option to select all of the plugins with available updates
7. Click on **Download now and install after restart**
8. When the updates are completed restart the master.

Now install the Blue Ocean and Pipeline Utility Steps plugins and their dependencies.

1. Click on the **Manage Jenkins** link
2. Click on the **Manage Plugins** link
3. Click on the **Available** tab
4. Select the **Blue Ocean** and **Pipeline Utility Steps** plugins and click on **Download now and install after restart**
5. Restart the master

### Setup Shared Library

In exercise 1.7 of the workshop participants will use a function contained within a Global Pipeline Library. Prior to running that exercise you need to add the library to the Jenkins Master using the following steps:

1. Click on the **Manage Jenkins** link
2. Click on **Configure System**
3. Scroll down to **Global Pipeline Libraries** and click on **Add**
4. Enter the following in the form fields:
   - Name: ```SharedLibs```
   - Default version: ```master```
5. Select **Modern SCM**
6. Select **Github**
7. For **Credentials** select **Add** and fill out a new **Username with password:
	- Username: ```PipelineHandsOn```
	- Password: Your Github token
	- ID: PipelineHandsOn-ID
	- Description: can be left blank
8. Select your newly added Credential from the dropdown list
9. Enter **PipelineHandsOn** in the **Owner** text box
10. Select **shared-libraries** from the **Repository** drop down
11. Click on **Save**

### Add test-user Credential

In exercise 1.3 participants will use the ```test-user``` credential in a pipeline so it needs to be added in advance. To add the credential:

1. From the Jenkins Master home page click on **Credentials**, then click on **System**, then **Global credentials (unrestricted)**.
2. Click on **Add Credentials**
3. In the **Add Credentials** form select **Username with password**, enter ```test-user``` for **Username**, **Password**, and the **ID** field.
4. Click on **OK** to save the new credential.

### Add User Accounts

If possible you should get a list of attendees in advance of the workshop and create login accounts for each to minimize the time required to setup the environment once the workshop starts. To keep things easy use the participants' first names as their user name and password.

### Send a Pre-Worshop Email to Participants

Prior to the workshop you should send out an email to participants to let them know what they will need to fully participate in the workshop. Below is a sample of the content you should send:

```
Dear Intro to Declarative Pipeline Workshop Participant:

In order to follow along with the hands on portion of the workshop students should have the following resources available to them:

  * Internet access
  * An account on Github.com and a basic understanding of how to use Github and Git to do things like fork a repository, edit files in the web UI, and create pull requests
  * A personal access token for your Github account (https://github.com/PipelineHandsOn/intro-to-declarative-pipeline/blob/master/Github-Personal-Access-Token.md) with the following permissions:
    - repo: all
    - admin:repo_hook: all
    - admin:org_hook
    - user: all
  * Access to the CloudBees Jenkins Enterprise Master hosted on AWS used for the workship (link to be provided the day of the workshop)

Thanks and I look forward to meeting working with everyone soon!

[Your name here obviously]
```

### Verify the Master is Configured Properly

It is always a good idea to test the master prior to the workshop to avoid any surprises. The best way to do that is to run through some of the exercises and verify the participant user accounts.