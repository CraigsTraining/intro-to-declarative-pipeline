# Exercise 1.1

In **Exercise 1.1** we will create a simple declarative pipeline directly within the Jenkins interface.

To create the new pipeline:

1. Log into Jenkins
2. Click on **New Item**
3. Type you name into the **Enter an item name** text box, click on **Folder**, and then click on the **OK** button.
4. Click on **Save** in the folder configuration screen to accept the default options.
5. Click on the **create new jobs** link.
6. Type **SimplePipeline** into the **Enter an item name** text box, click on **Pipeline**, and then click on the **OK** button.
7. Copy and paste the following code into the **Pipeline Script** text box:

```
pipeline {
   agent any
    
   stages {
      stage('Say Hello') {
         steps {
            echo 'Hello World!'   
         }
      }
   }
}
```
Click on **Save** and then click on **Build Now** to run your pipeline.

# Exercise 1.2

In **Exercise 1.2** we will update the pipeline we created in 1.1 to execute in a docker container. To update the pipeline:

1. Click on configure and update the ```agent``` portion of the pipeline to read:

```
   agent {
      docker { image 'maven:3.3-jdk-8' }
   }
```

# Exercise 1.3

For **Exercise 1.3** we are going to update our **SimplePipeline** job to demonstrate how to use environmental variables.

At the top of the pipeline insert the following code between the ```agent``` and ```stages``` blocks:  

```
   environment {
      MY_NAME = 'Mary'
   }
```

Then update the ```echo 'Hello World!'``` line to read ```echo "Hello ${MY_NAME}!" and run your build again to view the results.

We can also use environmental variables to import credentials. To demonstrate we will add the following line to our ```environment``` block:

```TEST_USER = credentials('test-user')```

We will also add the following ```echo``` steps within the ```steps``` of the Say Hello ```stage```:

```
            echo "${TEST_USER_USR}"
            echo "${TEST_USER_PSW}"
```

**Note**: After executing the build look at the console output and make note of the fact that the credential user name and password are masked when output via the echo command.

# Exercise 1.4

In **Exercise 1.4** we will alter our pipeline to accept external input in the form of a Parameter.

At the top of your pipeline insert the following block of code between the ```environment``` and ```stages``` blocks:

```
   parameters {
      string(name: 'Name', defaultValue: 'whoever you are', 
	     description: 'Who should I say hi to?')
   }
```
Then update the ```echo "Hello ${MY_NAME}!'``` line to read ```echo "Hello ${params.Name}!" and run your build again to view the results.

**Note**: Jenkins UI won't update properly when you save the pipeline to show the ```Build with parameters``` option so you need to run a build, view the results, and then return to the project to see the updated option.

# Exercise 1.5

For **Exercise 1.5** we are going to add a new stage after the **Say Hello** stage that will demonstrate how to ask interactively for user input.

Insert the following ``stage``` block into your pipeline:

```
      stage('Deploy') {
         steps {
            input 'Should I deploy?'
         }
      }
```

**Note**: To keep Jenkins from waiting indefinitely for a user response you should wrap you input steps in a ```timeout``` like shown below:

```
            timeout(time: 1, unit: 'MINUTES') {
               input 'Should I deploy?'
            }
```

