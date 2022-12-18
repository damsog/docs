# **Continuous Integration / Continuous Deployment - guide**

## **Description**


## **Bitbucket Pipelines**

### *Runners*
A runner is a container or a system that is the actual one tasked to run the operations set in the pipeline. <br>
We declare a manifest with the orders we want to run or execute and runners take them and execute them.<br>
<br>
These runners can be hosted either on bitbucket or on a personal server, although bitbucket hosting is not free after a certain<br>
ammount of computational hours.
<br>
To create a Runner go to the repository settings (You will need administrator permissions to access this section).
go all the way down to the pipelines section and then click on runners.<br>
To create a runner you first have to enable two step authentication for bitbucket.<br>
Click Add Runner. Here select the OS for the runner, a name to identify it and some labels which we will use later to<br>
tell a script on which runner operate.<br>
Before Finishing we are given a command to run our runner on our machine. it is actually a docker container<br>
with some ids to uniquelly identify it, so copy and save it, we may need it if our container stops.<br>
With our command we can now run it, and verify on bitbucket that it's online.<br>
Now stop it and change the flag -it for -d to run it detached in the background.

### *Repository Variables*


## **GitLab Pipelines**

## **Github Actions**
