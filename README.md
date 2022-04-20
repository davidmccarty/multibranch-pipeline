# multibranch-pipeline-demo
Jenkins Multibranch Pipeline Example Repo

https://devopscube.com/jenkins-multibranch-pipeline-tutorial/

## Create repos in gitea
1. Fork the pipeline and application repos to github
2. Mirror them into gitea
3. Use settings to convert to regular repos (i.e. break the mirror)


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
3. Add the gitea repo as source and create a credential
4. Set option for strategy = 'only branches that are also files as PRs'

## Update the pipeline
1. Checkout http://git.mk-devops.local/dmccarty/multibranch-pipeline.git
2. Modify pipeline file to include kubernetes container
    ```yaml
    pipeline {

    agent {
        kubernetes {
        yaml '''
        apiVersion: v1
        kind: Pod
        spec:
            containers:
            - name: alpine
            image: alpine:3.14
            command:
            - cat
            tty: true
            serviceAccount: jenkins-builder
        '''
        }
    }
    ```
3. Update the URL for teh petstore application to pull from internal gitea service
   ```yaml
    stage('Code Checkout') {
      steps {
        checkout([
            $class: 'GitSCM',
            branches: [[name: '*/main']],
            userRemoteConfigs: [[url: 'http://gitea-http.gitea.svc.mk-devops.local:3000/dmccarty/spring-petclinic.git']]
        ])
      }
    }
   ```
4. Create pull request from branch 'feature' --> 'develop'
