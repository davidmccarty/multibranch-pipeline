# multibranch-pipeline-demo
Jenkins Multibranch Pipeline Example Repo

https://devopscube.com/jenkins-multibranch-pipeline-tutorial/

## Create repos in gitea
1. Fork the pipeline and application repos to github
2. Mirror them into gitea


## Setup Gitea plugin
1. Install plugin for gitea SCM
2. In Jenkings server set create server definition
    ```
    name = gitea
    URL = http://gitea-http.gitea.svc.mk-devops.local:3000
    ```

## Create multibranch pipeline in Jenkins
1. New folder = Petstore
2. Create new item --> multibranch pipeline and copy the jenkins file source from here
3. Add the gitea repo
