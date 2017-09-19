# Create a Github Personal Access Token

The following instructions cover how to create a Github Personal Access Token that you will use within Jenkins to connect pipelines, multibranch pipelines, and Github organization projects to your Github repositories.

1. Log into Github.com
2. Select the **Settings** option from the profile menu in the upper right hand corner of the browser window
3. Click on **Personal access tokens** under **Developer settings**
4. Fill out a description for the token (e.g. **MyJenkinsToken**)
5. Select the following options:
  * repo
  * admin:org
  * admin:repo_hook
  * admin:org_hook
  * user
6. Click on **Generate Token**
7. As the success message says: **Make sure to copy your new personal access token now. You won’t be able to see it again!**  
