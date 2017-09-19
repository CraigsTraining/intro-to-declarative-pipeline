# Exercise 2.1

In **Exercise 2.1** we are going to start by forking an existing Github project that has multiple branches and Jenkinsfiles in each branch.

But first let's create a Github organization to fork the repo into:

1. On Github navigate to **Organizations**: https://github.com/settings/organizations (after logging in)
2. Click on **New Organization**
3. Fill in the **Organization Name**, **Billing Email**, and click on **Create Organization**

Now lets fork the repo into the new organization:

1. Navigate to the rest server application we are going to work with: https://github.com/CraigsTraining/sample-rest-server
2. Click on **Fork**
3. Select the **Organization** you want to fork into

# Exercise 2.2

In this exercise we are going to create a Github Organization project from our newly forked repository.

**Note**: You need to have a Github personal access token before proceeding.

First let's add your Github credentials to the Jenkins' Credentials manager:

1. Click on **Credentials**
2. Click on **Folder**
3. Click on **Global Credentials (Unrestricted)**
4. Click on **Add Credentials**
5. Fill out the form (**Username with password**)
  - **Username**: The Github organization name
  - **Password**: Your Github personal access token
  - **ID**: Create an ID for your credentials (something like **yourorg-id**)
  - **Description**: Can be left blank if you want
6. Click on **OK**

Now let's create the Github Organization project:

1. Navigate back to your personal folder in Jenkins
2. Click on **New Item**
3. Enter your organization name as the **Item Name**
4. Select **GitHub Organization**
5. Click **Ok**
6. Select the credentials you created above from the **Credentials** drop down
7. Select **All** from the **Strategy** drop down under **Discover Branches**
8. Click **Save**

Once you click on save Jenkins will search your organization for any projects with Jenkinsfiles in them, import those projects as Multibranch projects, and begin building each branch with a Jenkinsfile in it.




