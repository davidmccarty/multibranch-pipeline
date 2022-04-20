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

  options {
    buildDiscarder logRotator(
                daysToKeepStr: '1',
                numToKeepStr: '1'
        )
  }

  stages {

    stage('Cleanup Workspace') {
      steps {
        cleanWs()
        sh 'echo Cleaned Up Workspace For Project'
      }
    }

    stage('Code Checkout') {
      steps {
        checkout([
            $class: 'GitSCM',
            branches: [[name: '*/main']],
            userRemoteConfigs: [[url: 'http://gitea.gitea-http.svc.mk-devops.local:3000/dmccarty/spring-petclinic.git']]
        ])
      }
    }

    stage(' Unit Testing') {
      steps {
        sh 'echo Running Unit Tests'
      }
    }

    stage('Code Analysis') {
      steps {
        sh 'echo Running Code Analysis'
      }
    }

    stage('Build Deploy Code') {
      when {
        branch 'develop'
      }
      steps {
        sh 'echo Building Artifact'

        sh 'echo Deploying Code'
      }
    }

  }
}
