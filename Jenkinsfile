pipeline {
    agent none
    parameters {
        choice(name: 'ENV', choices: ['dev', 'test', 'prod', 'release'])
    }
    stages {
        stage('build') {
            agent {
                label 'slave-dev-label'
            }
            when {
                expression {
                    return params.ENV == 'release'
                }
            }
            steps {
                script {
                    if (params.ENV == 'release') {
                         withCredentials([usernamePassword(credentialsId: 'dockerhub-secret', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                            sh '''
                                docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}
                                docker build -t 712199425/lab2iti:v${BUILD_NUMBER} .
                                docker push 712199425/lab2iti:v${BUILD_NUMBER}
                                echo ${BUILD_NUMBER} > ../build.txt
                            '''
                        }
                    }
                }
            }
        }
        stage('deploy') {
            agent {
                label {
                    switch (params.ENV) {
                        case 'dev':
                            return 'slave-dev-label'
                        case 'test':
                            return 'slave-test-label'
                        case 'prod':
                            return 'slave-prod-label'
                        default:
                            return 'slave-release-label'
                    }
                }
            }
            when {
                expression {
                    return params.ENV != 'release'
                }
            }
            steps {
                script {
                    if (params.ENV == 'dev' || params.ENV == 'test' || params.ENV == 'prod') {
                        withCredentials([file(credentialsId: 'iti-smart-kubeconfig', variable: 'KUBECONFIG_ITI')]) {
                            sh '''
                                export BUILD_NUMBER=$(cat ../build.txt)
                                mv Deployment/deploy.yaml Deployment/deploy.yaml.tmp
                                cat Deployment/deploy.yaml.tmp | envsubst > Deployment/deploy.yaml
                                rm -f Deployment/deploy.yaml.tmp
                                kubectl apply -f Deployment --kubeconfig ${KUBECONFIG_ITI} -n ${ENV}
                            '''
                        }
                    }
                }
            }
        }
    }
}
