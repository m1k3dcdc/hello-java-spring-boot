// Based on:
// https://raw.githubusercontent.com/redhat-cop/container-pipelines/master/basic-spring-boot/Jenkinsfile



// The name you want to give your Spring Boot application
// Each resource related to your app will be given this name
//appName = "hello-java-spring-boot"

pipeline {
    // Use the 'maven' Jenkins agent image which is provided with OpenShift 
    //agent { label "maven" }
    agent any
    
    stages {
        stage("Checkout") {
            steps {
                checkout scm
            }
        }
        stage("Docker Build") {
            steps {
                // This uploads your application's source code and performs a binary build in OpenShift
                // This is a step defined in the shared library (see the top for the URL)
                // (Or you could invoke this step using 'oc' commands!)
                //binaryBuild(buildConfigName: appName, buildFromPath: ".")
                //sh 'mvn clean install' 
                //sh 'oc start-build hello-java-spring-boot --from-file=target/hello-java-spring-boot.jar --follow'
                sh 'oc start-build hello-java-spring-boot --from-dir=.  --follow'
                //openshift.selector("bc", "hello-java-spring-boot").startBuild("--follow") 
                //openshift.selector("bc", "hello-java-spring-boot").startBuild("--from-dir=.", "--follow")
            }
        }

        // You could extend the pipeline by tagging the image,
        // or deploying it to a production environment, etc......
        stage('Deploy to OpenShift') {
            steps {
                script {
                    //sh "oc rollout latest deploy/hello-java-spring-boot -n mavc23-dev"
                    sh "oc apply -f ."
                }
            }
        }
    }
}
