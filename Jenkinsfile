pipeline{
    agent {
        label: 'java_from_python_node'
    }

    stages{
        stag('pull'){
            steps {
                git 'https://github.com/hishiry/for_jenkins_agent_java.git'
            }
        }
        stag('build'){
            steps {
                sh 'mvn clean package'
            }
            post{
                sucess{
                    archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
                }
            }
        }
        stag('test'){
            steps {
                sh 'mvn clean test -Dmaven.test.failure.ignore=true'
            }
            post{
                sucess{
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
    }
}