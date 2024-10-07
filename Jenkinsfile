pipeline {
    agent any
    stages {
        stage("build") {
            steps {
                echo "build..."
                docker -composer build
            }
        }
        stage("Tests") {
            steps {
                echo " test..."
                docker-compose -f docker-compose-tests.yml up --exit-code-from SumaTest SumaTest
            }
        }
    }
        stage("deploy") {
            steps {
                echo " deploy..."
                docker-compose up -d --forse-recreate Suma
                docker-compose up -d --forse-recreate SitioWeb
            }
        }
    }
}
