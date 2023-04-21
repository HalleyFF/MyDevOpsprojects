pipeline {
  agent {
    kubernetes {
      //cloud 'kubernetes'
      defaultContainer 'kaniko'
      yaml '''
        kind: Pod
        spec:
          containers:
          - name: kaniko
            image: gcr.io/kaniko-project/executor:v1.6.0-debug
            imagePullPolicy: Always
            command:
            - sleep
            args:
            - 99d
            volumeMounts:
              - name: jenkins-docker-cfg
                mountPath: /kaniko/.docker
          volumes:
          - name: jenkins-docker-cfg
            projected:
              sources:
              - secret:
                  name: regcred
                  items:
                    - key: .dockerconfigjson
                      path: config.json
'''
    }
  }
  stages {
    stage('OCI Image BnP') {
      steps {
        git branch: 'main', url: 'https://github.com/fdjapi/Kubernetes-jenkins.git'
        container('kaniko') {
          sh "/kaniko/executor --force -f ${WORKSPACE}/Dockerfile -c ${WORKSPACE} --insecure --skip-tls-verify --cache=true --destination=docker.io/fdjapi10/dsodemo:v2"
        }
      }
    }
  }
}
