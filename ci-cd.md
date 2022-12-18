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

Before starting with our pipeline we have to prepare our runner.
To create a Runner on bitbucket go to the repository settings (You will need administrator permissions to access this section).<br>
<p align="center">
  <img src="https://user-images.githubusercontent.com/46113808/208315661-49c2a4ad-f3af-430b-82d5-ca67434c27e7.png?raw=true" alt="runner1"/>
</p>
go all the way down to the pipelines section and then click on runners.<br>
<p align="center">
  <img src="https://user-images.githubusercontent.com/46113808/208313342-994d4df9-2ff6-4912-97a9-74e61fdd3531.png?raw=true" alt="runner2"/>
</p>

To create a runner you first have to enable two step authentication for bitbucket.<br>
Click Add Runner. Here select the OS for the runner, a name to identify it and some labels which we will use later to<br>
tell a script on which runner operate.<br>
Before Finishing we are given a command to run our runner on our machine. it is actually a docker container<br>
with some ids to uniquelly identify it, so copy and save it, we may need it if our container stops.<br>
With our command we can now run it, and verify on bitbucket that it's online.<br>
<p align="center">
  <img src="https://user-images.githubusercontent.com/46113808/208316009-0d7a629a-c94a-4c10-8d9b-5737d4885860.png?raw=true" alt="runner3"/>
</p>

Now stop it and change the flag -it for -d to run it detached in the background.

### *Repository Variables*


## [**GitLab Pipelines**](https://docs.gitlab.com/ee/ci/quick_start/)

## [**Github Actions**](https://docs.github.com/es/actions)
