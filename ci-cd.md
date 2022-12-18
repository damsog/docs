# **Continuous Integration / Continuous Deployment - guide**

## **Description**
An important part of Continuous integration and continuous deployment it's to automate the development cycle. from the coding up to the final step in deployment. <br>
Since most of the steps are repetitive, we can create pipielines to describe how are we going to run tests, build packages upload our package to a repository and then deploy on production. <br>
There are many solutions for this process, most of these solutions are offered by the repository platforms (Like gitlab, github, bitbucket, azure devops, etc) which eases the integration with the existing development cycle.<br>
<br>
A pipeline consists on a series of operations described on a yaml file, this file describes the jobs that should be executed, and it's activated when merging the code to a specific branch.<br>
The platform then sends a job request to a runner which is a system (Usually a container) provided by the pipelines platform, and the runner will execute them. <br>
#### *Runners*
A runner is a container or a system that is the actual one tasked to run the operations set in the pipeline. <br>
To describe the operations that we want to run we declare a manifest with the orders we want to run or execute and runners take them and execute them.<br>
Usually these runners can be hosted on the pipeline platform or self hosted on a private server, although platform hosting is not free after a certain ammount of computational hours.
<br>
<p align="center">
  <img src="https://user-images.githubusercontent.com/46113808/208314938-aaa12a1c-2c5d-44f7-96f6-eeed82f875ae.png?raw=true" alt="Pipeline"/>
</p>

## [**Bitbucket Pipelines**](https://support.atlassian.com/bitbucket-cloud/docs/get-started-with-bitbucket-pipelines/)

To describe our pipeline we use a manifest file in yaml format to explain the systen what we want. however, before starting with the manifest file lets prepare the environment and create a runner.

### *Deployment Environment*

Prepare your deployment environment, prepare your server, install the necesary dependencies for your application and also [docker](https://docs.docker.com/engine/install/ubuntu/) which will be necessary for the runner. you might also test your server manually before automating it with pipelines.

### *Runner*

Before starting with our pipeline we have to prepare our runner.
To create a Runner on bitbucket go to the repository settings (You will need administrator permissions to access this section).<br>
<p align="center">
  <img src="https://user-images.githubusercontent.com/46113808/208315661-49c2a4ad-f3af-430b-82d5-ca67434c27e7.png?raw=true" alt="runner1"/>
</p>
go all the way down to the pipelines section and then click on runners.<br>
<p align="center">
  <img src="https://user-images.githubusercontent.com/46113808/208313342-994d4df9-2ff6-4912-97a9-74e61fdd3531.png?raw=true" alt="runner2"/>
</p>

NOTE: To create a runner you first have to enable two step authentication for bitbucket.<br>
Click Add Runner. Here select the OS for the runner, a name to identify it and some labels which we will use later to<br>
tell a script on which runner operate.<br>
Before Finishing we are given a command to run our runner on our machine. it is actually a docker container<br>
with some ids to uniquelly identify it, so copy and save it, we may need it if our container stops.<br>
With our command we can now run it, and verify on bitbucket that it's online.<br>
<p align="center">
  <img src="https://user-images.githubusercontent.com/46113808/208316009-0d7a629a-c94a-4c10-8d9b-5737d4885860.png?raw=true" alt="runner3"/>
</p>

Now stop it and change the flag -it for -d to run it detached in the background.

### *Pipelines Manifest File*

Now that we have our runner and our deployment environment, lets describe a pipeline.<br>
Usually you would run tests, then build your application on another step, push it to a docker hub server, the on the deployment step you pull the updated image.<br>
For simplicity, let's just deploy and application (You can create the number of steps you want, the four mentioned above are just the most common ones)<br>
NOTE: We will refference a lot of variables with $VAR, these are environment variables, let's ignore that we haven't set these variables yet. we will do it after we set the pipeline manifest. <br>
For a bitbucket pipeline create a fine named bitbucket-pipelines.yml in the root of the repository of the project and Add the following:

```yaml
image: atlassian/default-image:3

pipelines:
  branches:
    master:
```

We start the file with a docker image. This container will run the jobs and each job is completly isolated from each other and they don't save state. <br>
Below, we start with the pipeline itself, we then describe which branches will serve as trigger, in this case, master.

```yaml
pipelines:
  branches:
    master:
      - step:
          name: Deploy
          runs-on:
            - 'self.hosted'
            - 'linux'
```

Now we describe a step (Like shown in the image above) to deploy the application and name it deploy (you can name it however you want). lets not pay attention to the runs-on: yet.

```yaml
image: atlassian/default-image:3

pipelines:
  branches:
    master:
      - step:
          name: Deploy
          runs-on:
            - 'self.hosted'
            - 'linux'
          script:
            - pipe: atlassian/ssh-run:0.2.2
              variables:
                SSH_USER: $SSH_USER
                SERVER: $SSH_HOST
                SSH_KEY: $SSH_KEY
                SERVER_PASS: $SERVER_PASS
                BITBUCKET_USER: $BITBUCKET_USER
                BITBUCKET_PASS: $BITBUCKET_PASS
```

Now we will tell the pipeline what to do, since we are going to run shell commands remotely we need to use another container (provided by bitbucket) to run ssh commands.

```yaml
image: atlassian/default-image:3

pipelines:
  branches:
    master:
      - step:
          name: Deploy
          runs-on:
            - 'self.hosted'
            - 'linux'
          script:
            - pipe: atlassian/ssh-run:0.2.2
              variables:
                SSH_USER: $SSH_USER
                SERVER: $SSH_HOST
                SSH_KEY: $SSH_KEY
                SERVER_PASS: $SERVER_PASS
                BITBUCKET_USER: $BITBUCKET_USER
                BITBUCKET_PASS: $BITBUCKET_PASS
                COMMAND: >
                  cd /home/publicidad/Projects/project_publicity/events && 
                  git pull https://$BITBUCKET_USER:$BITBUCKET_PASS@bitbucket.org/direcciondesarrollo-admin/events.git/ && 
                  ./gradlew bootJar &&
                  cp ./build/libs/crud_events-0.0.1-SNAPSHOT.jar ../deployment/events_service/events.jar &&
                  echo $SERVER_PASS | sudo -S systemctl restart events.service
```

Lastly we run shell commands to update the code, compile the application and deploy the app on the server itself.<br>

Save the file and push if you want, this will trigger the pipeline (If pipelines was enabled when creating the runner) However, since the variables are still not set he pipeline should fail, but dont worry about that yet.

<p align="center">
  <img src="https://user-images.githubusercontent.com/46113808/208320357-61fe7d81-1fda-40f9-95ea-0559752a7264.png?raw=true" alt="variables"/>
</p>

Now lets Define the variables.

### *Repository Variables*

The pipeline system allows us to create environment variables that only the runner will see when running the jobs. it's a goob practice to not simply put accesses and names in the pipeline manifest, but to reference them. let's see how.<br>
Go to Repository Variables in the pipelines section.<br>
<p align="center">
  <img src="https://user-images.githubusercontent.com/46113808/208318416-096ecaa2-9a3f-4bcc-a9a0-9af8009becdc.png?raw=true" alt="variables"/>
</p>

Here you can add anything you want, like ssh accesses, docker hub acceses, keys, aplication names etc. just be sure to enable secured for keys and passwords.
<p align="center">
  <img src="https://user-images.githubusercontent.com/46113808/208318485-01248068-e3fe-4c1c-81e3-83c36ffbd004.png?raw=true" alt="variables"/>
</p>

Lets create SSH_USER, SSH_HOST, APPLICATION_NAME, DOCKER_USER, DOCKER_PASSWORD. (Dont make SSH_KEY yet, this one is a little different, we will make it in the next section)<br>
<p align="center">
  <img src="https://user-images.githubusercontent.com/46113808/208318699-ce29f145-a7ec-4317-9143-ae944a22d803.png?raw=true" alt="variables"/>
</p>
Now we can refference the value of these variables on our pipeline manifest using $VAR_NAME

### *SSH Access to the server*

Since we are automating our development and deployment cycle we need to grant server access to our runner (even if it runs on the server since it's a container). <br>
For that purpose we should create an [ssh-key pair](https://www.ssh.com/academy/ssh/public-key-authentication)<br>
we will use the private key to access to the server matching it to the public key that we will give to the server.
<p align="center">
  <img src="https://user-images.githubusercontent.com/46113808/208317179-f373d59a-3d39-46c3-a4c8-b5bac6fb40bd.png?raw=true" alt="ssh-key"/>
</p>

Go to your LOCAL machine (NOT the server). Create a key-pair in your own machine with the following command
```sh
ssh-keygen
```

Give it a name and a location if you want. normally it will be appear on ```~/.ssh/```

There should be 2 files. one file without extension (private key) and the other with .pub extension (public key).

Now lets move to the SERVER. Move or copy the public key to the server.<br>
Now check if the ```~/.ssh``` folder exists:

```sh
ls -l ~/.ssh/
```

If the directory is non-existent, create the folder:

```sh
mkdir ~/.ssh
```

Next, change the permissions with:

```sh
chmod 700 ~/.ssh
```

Next, Create a file called authorized_keys in the ~/.ssh directory:

```sh
touch authorized_keys
```

Change the permissions:

```sh
chmod 600 ~/.ssh/authorized_keys
```

Add the public key to authorized keys:

```sh
cat <path/to/public/key.pub> >> ~/.ssh/authorized_keys
```

Now go back to the LOCAL machine and test the connection

```sh
ssh -i <path/to/private/key> -p <port> <user>@<host>
```

Now we need to add the key to our pipeline environment so the runner can use it.<br>
The easiest way is to create a new pipeline variable and paste the key there, but for that we need to encode our key on base64 so we can use a long string instead of a file.<br>

Go to your LOCAL machine again and enter:

```sh
base64 -w 0 < <path/to/private/key>
```

Copy the output and save it as a pipeline variable (secured) the same as the previous variables
<p align="center">
  <img src="https://user-images.githubusercontent.com/46113808/208318797-ff26ec75-b379-4e04-9d9e-8aa528e2aec8.png?raw=true" alt="ssh-key"/>
</p>

### *Finishing Up*

Now that we defined our variables, let's check that everything it correctly refferenced in the ```bitbucket-piplines.yml``` file. then let's move to the pipelines tab again and click rerun to the failed pipeline, or push any changes to the master branch on bitbucket and it should trigger the pipeline.
<p align="center">
  <img src="https://user-images.githubusercontent.com/46113808/208320617-c7145ea1-7593-4646-9659-e7aff0709f87.png?raw=true" alt="ssh-key"/>
</p>


## [**GitLab Pipelines**](https://docs.gitlab.com/ee/ci/quick_start/)

## [**Github Actions**](https://docs.github.com/es/actions)
