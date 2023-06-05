pipeline{
    agent none

    stages {
        stage('build') {
            agent { label 'slave-release-label' }
            steps {
                echo 'build'
                script {
                    if (BRANCH_NAME == "release") {
                        withCredentials([usernamePassword(credentialsId: 'dockerhub-secret', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                            sh '''
                                docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}
                                docker build -t 712199425/lab2iti:v${BUILD_NUMBER} .
                                docker push 712199425/lab2iti:v${BUILD_NUMBER}
                                echo ${BUILD_NUMBER} > ../build.txt
                            '''
                        }
                    } else {
                        echo "user chose ${BRANCH_NAME}"
                    }
                }
            }
        }
        stage('deploy') {
            steps {
                script {
                    def env = ''
                    if (BRANCH_NAME == 'dev') {
                        env = 'slave-dev-label'
                    } else if (BRANCH_NAME == 'test') {
                        env = 'slave-test-label'
                    } else if (BRANCH_NAME == 'prod') {
                        env = 'slave-prod-label'
                    } else {
                        env = 'slave-release-label'
                    }
                }
                agent {
                    label env
                }
            }
            steps {
                echo 'deploy'
                script {
                    if (BRANCH_NAME == "dev" || BRANCH_NAME == "test" || BRANCH_NAME == "prod") {
                        withCredentials([file(credentialsId: 'iti-smart-kubeconfig', variable: 'KUBECONFIG_ITI')]) {
                            sh '''
                                export BUILD_NUMBER=$(cat ../build.txt)
                                mv Deployment/deploy.yaml Deployment/deploy.yaml.tmp
                                cat Deployment/deploy.yaml.tmp | envsubst > Deployment/deploy.yaml
                                rm -f Deployment/deploy.yaml.tmp
                                kubectl apply -f Deployment --kubeconfig ${KUBECONFIG_ITI} -n ${BRANCH_NAME}
                            '''
                        }
                    }
                }
            }
        }
    }
}
