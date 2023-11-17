pipeline {
    agent any
    
    parameters {
        string(name: 'GIT_URL', defaultValue: 'https://github.com/krishnamsg/hanvi-shop.git', description: 'Enter Git URL')
        string(name: 'GIT_BRANCH', defaultValue: 'master', description: 'Enter Git Branch')
        string(name: 'TOMCAT_IP', defaultValue: '10.10.0.3:8090', description: 'Enter Tomcat Server IP with port')
        string(name: 'TOMCAT_USERNAME', defaultValue: 'deployer', description: 'Enter Tomcat Username')
        password(name: 'TOMCAT_PASSWORD', defaultValue: 'deployer', description: 'Enter Tomcat Password')
        string(name: 'APP_CONTEXT', defaultValue: 'myapp', description: 'Enter App Context')
    }
     environment {
        TOMCAT_HOME = "/opt/tomcat-9"
        //WAR_FILE = "your-application.war"
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Clean workspace and checkout the selected branch
                    deleteDir()
                    checkout([$class: 'GitSCM', branches: [[name: "refs/heads/${params.GIT_BRANCH}"]], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: params.GIT_URL]]])
                }
            }
        }

        stage('Build') {
            steps {
                // Add build steps if needed
                sh 'mvn clean install'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    // Deploy to the specified Tomcat server and app context
                    def tomcatIpWithPort = params.TOMCAT_IP
                    def tomcatUsername = params.TOMCAT_USERNAME
                    def tomcatPassword = params.TOMCAT_PASSWORD
                    def appContext = params.APP_CONTEXT

                    // Split the IP and port
                    def (tomcatIp, tomcatPort) = tomcatIpWithPort.split(':')

                     // Remove old application files
                    sh "sudo rm -rf ${TOMCAT_HOME}/webapps/${appContext}*"
                    sleep 10s

                    // Add deployment steps based on your deployment strategy
                    // For example, use curl or deploy the war file to the Tomcat webapps directory
                    
                    sh "curl --user ${tomcatUsername}:${tomcatPassword} --upload-file target/*.war http://${tomcatIp}:${tomcatPort}/manager/text/deploy?path=/${appContext}"
                }
            }
        }
    }
}
