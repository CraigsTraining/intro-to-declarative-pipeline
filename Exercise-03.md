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
