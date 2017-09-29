# Exercise 2.1

In **Exercise 2.1** we are going to start by forking an existing Github project that has multiple branches and Jenkinsfiles in each branch.

But first let's create a Github organization to fork the repo into:

1. On Github navigate to **Organizations**: https://github.com/settings/organizations (after logging in)
2. Click on **New Organization**
3. Fill in the **Organization Name**, **Billing Email**, and click on **Create Organization**

Now lets fork the repo into the new organization:

1. Navigate to the rest server application we are going to work with: https://github.com/PipelineHandsOn/sample-rest-server
2. Click on **Fork**
3. Select the **Organization** you want to fork into

# Exercise 2.2

In this exercise we are going to create a Github Organization project from our newly forked repository.

**Note**: You need to have a Github personal access token ([Github-Personal-Access-Token.md](Github-Personal-Access-Token.md)) before proceeding.

First let's add your Github credentials to the Jenkins' Credentials manager:

1. Navigate back to your personal folder in Jenkins
2. Click on **Credentials**
3. Click on **[YourFolderName]** under **Stores Scoped to [YourFolderName]**
4. Click on **Global Credentials (Unrestricted)**
5. Click on **Add Credentials**
6. Fill out the form (**Username with password**)
  - **Username**: The Github organization name
  - **Password**: Your Github personal access token
  - **ID**: Create an ID for your credentials (something like **yourorg-id**)
  - **Description**: Can be left blank if you want
7. Click on **OK**

Now let's create the Github Organization project:

1. Click on **New Item**
2. Enter your organization name as the **Item Name**
3. Select **GitHub Organization**
4. Click **Ok**
5. Select the credentials you created above from the **Credentials** drop down
6. Select **All** from the **Strategy** drop down under **Discover Branches**
7. Click **Save**

Once you click on save Jenkins will search your organization for any projects with Jenkinsfiles in them, import those projects as Multibranch projects, and begin building each branch with a Jenkinsfile in it.

When the project was created it also should have created webhooks in Github. Verify that the webhooks were created in Github by checking **Webhooks** within your organization's Github **Settings**.

# Exercise 2.3

In this exercise we are going to edit the Jenkinsfile file in the **development** branch of our project to add a branch specific stage.

1. Within your **sample-rest-server** project select the **development** branch from the **Branch** drop down menu
2. Click on the **Jenkinsfile** in the file list
3. Click on the **Edit this file** button (pencil)
4. Insert the following stage after the existing **build** stage:

```
      stage('Development Tests') {
         when {
            branch 'development'
         }
         steps {
            echo "Run the development tests!"
         }
      }
```

5. Fill out the commit information, select 'Commit directly to the development branch.', and click on **Commit Changes**

Notice how after you commit your changes the Github web hooks trigger a build of the development branch in Jenkins.

# Exercise 2.4

In this exercise we are going to edit the development branch's Jenkinsfile again but make our commit against a feature branch and user a pull request to merge the edits into our development branch.

1. Click on the **Edit this file** button (pencil)
2. Insert the following stage after the existing **build** stage:

```
      stage('Masters Tests') {
         when {
            branch 'master'
         }
         steps {
            echo "Run the master tests!"
         }
      }
```

3. Fill out the commit information, select 'Create a new branch for this commit and start a pull request.' and click on **Propose file change**
4. Flip back to your Jenkins job and notice that the new feature branch appears in your projects
5. Return back to the Github **Open a pull request** page
6. Click on the **Create pull request** button
7. Go to your Jenkins job and notice that that the PR has been added to the Pull Requests tab
8. In Github click on **Merge pull request** and then **Confirm** to close the PR and merge the results into the development branch
9. Optionally you can also delete the feature branch you created

Finally, we should merge our work into our master branch to verify that our changes work there:

1. Return back to your repository's main page where you will be on the master branch by default
2. Click on **New pull request**
3. Select your base fork (not the project we forked from)
4. Compare **master** to **development**
5. Click **View pull request**
6. Click **Merge pull request**
7. Click **Confirm merge**

Notice how after you merge your changes into master the Github web hooks trigger a build of the master branch in Jenkins.



