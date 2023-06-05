pipeline { 
    agent { label 'iti-smart-slave-paa' } 
    // pipeline اللي هترن عليه node دا اسم الlabel المحطوط على
    stages {
        stage('test') {
            steps {
                echo 'Hello World' 
                sh "ls"
            }
        }
        stage('build') {
            steps {
                echo 'Hello World'
                script {
                    withCredentials([usernamePassword(credentialsId:'dockerhub-secret',usernameVariable:'DOCKER_USERNAME',passwordVariable:'DOCKER_PASSWORD')])
                    sh '''
                        docker build -t 712199425/lab2ITI:v1 .
                        docker login -u ${DOCKER_USERNAME} -p {DOCKER_PASSWORD}
                        docker push 712199425/lab2ITI:v1
                        echo ${BUILD_NUMBER}
                    '''
                    }
                }
        }
        stage('deploy') {
            steps {
                sh "echo 'Hello World'"
            }
        }
    }
}
