// Based on:
// https://raw.githubusercontent.com/redhat-cop/container-pipelines/master/basic-spring-boot/Jenkinsfile
pipeline {
    agent any
    
    stages {
        stage("Checkout") {
            steps {
                checkout scm
            }
        }

        stage('Create Container Image') {
          steps {
            echo 'Create Container Image..'
            
            script {
              openshift.withCluster() {
                openshift.withProject("mavc23-dev") {
                    def buildConfigExists = openshift.selector("bc", "hello-java-spring-boot").exists()
    
                    if(!buildConfigExists){
                        openshift.newBuild("--name=hello-java-spring-boot", "--docker-image=docker.io/m1k3pjem/hello-java-spring-boot", "--binary")
                    }    
                    openshift.selector("bc", "hello-java-spring-boot").startBuild("--from-dir=.", "--follow")    
                }    
              }
            }
          }
        }
        
        // You could extend the pipeline by tagging the image,
        // or deploying it to a production environment, etc......
        stage('Deploy to OpenShift') {
            steps {
                script {
                    //sh "oc rollout latest deploy/hello-java-spring-boot -n mavc23-dev"
                    //sh "oc create --save-config -f ."
                    sh "oc apply -f . -n mavc23-dev"
                }
            }
        }
    }
}
