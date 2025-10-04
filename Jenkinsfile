// Jenkinsfile — 7‑stage CI/CD 
pipeline {
  agent any

  triggers { pollSCM('H/5 * * * *') }  

  options { timestamps() }

  environment {
    APP_NAME = 'sample-app'
    STAGING_URL = 'http://staging.example.com'   
    PROD_URL    = 'http://prod.example.com'     
  }

  stages {
    stage('Stage 1: Build') {
      steps {
        echo 'Building the project with a build tool (e.g., Maven/Gradle/npm).'
      }
    }

    stage('Stage 2: Unit & Integration Tests') {
      steps {
        echo 'Running unit tests (e.g., JUnit) and integration tests.'
      }
      post {
        always {
          echo 'Publishing test results (e.g., JUnit reports).'
        }
      }
    }

    stage('Stage 3: Code Analysis') {
      steps {
        echo 'Static analysis with SonarQube/Checkstyle/PMD.'
      }
    }

    stage('Stage 4: Security Scan') {
      steps {
        echo 'Security scan for vulnerabilities (OWASP Dependency-Check/Snyk).'
      }
      post {
        always {
          echo '(Would publish security report artifacts here)'
        }
      }
    }

    stage('Stage 5: Deploy to Staging') {
      steps {
        echo 'Deploying build artifact to STAGING (e.g., AWS EC2/Docker/K8s).'
      }
    }

    stage('Stage 6: Integration Tests on Staging') {
      steps {
        echo "Running end‑to‑end/API/UI tests against ${STAGING_URL} (e.g., Postman/Selenium/Cypress)."
      }
    }

    stage('Stage 7: Deploy to Production') {
      when { branch 'main' }          
      steps {
        echo 'Promoting the release to PRODUCTION (e.g., AWS EC2/K8s).'
        echo "Production URL: ${PROD_URL}"
      }
    }
  }

  post {
    success { echo 'Pipeline completed successfully.' }
    failure { echo 'Pipeline failed — check stage logs.' }
  }
}
