pipeline{
    agent {label 'agent1'}
    options{
        buildDiscarder(logRotator(numToKeepStr: '4'))
        disableConcurrentBuilds()
    }
    parameters{
        booleanParam(name: "docker", defaultValue: true, description: "")
        booleanParam(name: "trivy", defaultValue: false, description: "")
        booleanParam(name: "cache", defaultValue: false, description: "")
    }

stages{
    stage("Clean Docker"){
        when{ expression{ return params.docker }}
        steps{
        sh '''
        docker images
        docker system prune -a -f
        cd ~/jenkins/ && ls ./workspace/
        docker images
        '''
    }
}
    stage("Clean trivy"){
         when{ expression{ return params.trivy }}
        steps{
            script{
                sh '''trivy image --reset
                trivy image --clear-cache
                trivy --cache-dir ~/trivy/ image --download-java-db-only
                trivy --cache-dir ~/trivy/ image --download-db-only
                '''
            }
        }
    }
    stage("Clean Jenkins Cache"){
         when{ expression{ return params.cache }}
        steps{
            script{
                sh '''ls ~/jenkins/
                cd ~/jenkins/caches/
                rm -rf ./*
                '''
            }}}
}
}
