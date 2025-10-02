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

        stage('Deploy') {
          steps {
            echo 'Deploying....'
            script {
              openshift.withCluster() {
                openshift.withProject("mavc23-dev") {
    
                  def deployment = openshift.selector("dc", "hello-java-spring-boot")
    
                  if(!deployment.exists()){
                    openshift.newApp('hello-java-spring-boot', "--as-deployment").narrow('svc').expose()
                  }
    
                  timeout(5) { 
                    openshift.selector("dc", "hello-java-spring-boot").related('pods').untilEach(1) {
                      return (it.object().status.phase == "Running")
                      }
                    }
                }
              }
            }
          }
        }

    }
}
