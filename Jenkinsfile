pipeline{
    agent{
        label any
    }

    tools {
        maven 'Maven_3.9.6'
    }

    stages{
        stage('SCM Checkout'){
            steps{
                checkout changelog: false, poll: false, scm: scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/gayathri-117/java-maven-war-app.git']])
                
            }

        }

        stage('Maven-Build'){
            steps{
                sh 'mvn clean install'
            }
        }

        stage('Sonar Scan'){
            steps{
                withSonarQubeEnv("SonarQube") {
                    sh "${tool("Sonar_5.0.1")}/bin/sonar-scanner \
                    -Dsonar.host.url=http://13.233.193.182:9000/ \
                    -Dsonar.login= sqp_a6da37b8db97ca9714e6cffe83bf21ba0ee1e936 \
                    -Dsonar.java.binaries =target \
                    -Dsonar.projectKey= java-maven-war-app"
                
                }
            }
        }

        stage('Nexus Upload'){
            steps{
                sh 'mvn -s settings.xml clean deploy'
            }
        }

    }
}
