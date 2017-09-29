# Exercise 3.1

In this exercise we are going to quickly create a new pipeline to demonstrate how Checkpoints work and how end users can interact with Checkpoints once a job as been built. To test this create a new pipeline job in your personal folder copying and pasting the following code into the Pipeline Script textbox:

```
pipeline {
   agent any
   stages {
      stage('One') {
         steps {
            echo 'Stage One - Step 1'
         }
      }
      stage('Checkpoint') {
         agent none
         steps {
            checkpoint 'Checkpoint'
         }
      }
      stage('Two') {
         steps {
            echo 'Stage Two - Step 1'
         }
      }
   }
}
```

After saving the job click on **Build Now** to run it.

When the job has completed running you will see a **Resume** icon in the build's **Stage View**. Clicking on the **Resume** icon gives you the ability to:

* **Delete** - Delete the cached artifacts and configuration for that build;
* **Restart** - Restart the build from the checkpoint.

**Note**: Deleting a checkpoint doesn't make the **Resume** icon vanish.

# Exercise 3.2

In order to complete the following exercise you will need to fork the following repositories into your Github organization:

* https://github.com/PipelineHandsOn/maven-project
* https://github.com/PipelineHandsOn/custom-marker-files

Once those repositories are forked:

1. Click on the Github organization project you created in **Exercise 2.2**.
2. Click on **Configure**
3. Under **Project Recognizers** select **Custom Script**
4. In **Marker file** type ```pom.xml```
5. Under **Pipeline** - **Definition** select **Pipeline script from SCM**
6. Select **Git** from **SCM**
7. In **Repository URL** enter: ```https://github.com/PipelineHandsOn/custom-marker-files```
8. Select your credentials from the **Credentials** menu
9. In **Script path** enter: ```Jenkins-pom```
