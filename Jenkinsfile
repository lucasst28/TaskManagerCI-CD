pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/lucasst28/TaskManagerCI-CD.git', branch: 'main'
            }
        }

        stage('Build') {
            steps {
                sh 'dotnet build src/TaskManagerAPI/TaskManagerAPI.csproj -c Release'
            }
        }

        stage('Unit Tests') {
            steps {
                sh 'dotnet test tests/TaskManagerAPI.UnitTests/TaskManagerAPI.UnitTests.csproj --logger "trx;LogFileName=unit-tests.xml" --configuration Release'
            }
            post {
                always {
                    junit '**/*.trx'  // Gera relatório de testes no Jenkins
                }
            }
        }

        stage('Integration Tests') {
            steps {
                sh 'dotnet test tests/TaskManagerAPI.IntegrationTests/TaskManagerAPI.IntegrationTests.csproj --logger "trx;LogFileName=integration-tests.xml" --configuration Release'
            }
            post {
                always {
                    junit '**/*.trx'
                }
            }
        }
    }

    post {
        failure {
            // Exemplo: Notificar via e-mail ou Slack
            emailext body: 'Build falhou! Verifique: ${BUILD_URL}', subject: 'ERRO no Pipeline: ${JOB_NAME}', to: 'seu-email@example.com'
        }
    }
}