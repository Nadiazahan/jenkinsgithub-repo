// Jenkinsfile — 7‑stage CI/CD 
pipeline {
  agent any

  triggers { pollSCM('H/5 * * * *') }  // every ~5 mins

  options { timestamps() }

  environment {
    APP_NAME = 'sample-app'
    STAGING_URL = 'http://staging.example.com'   // placeholder
    PROD_URL    = 'http://prod.example.com'      // placeholder
  }

  stages {
       stage('Stage 0: Git Connection Check') {
    steps {
      echo 'Checking Git connection and showing latest commit info...'
      sh 'git log -1 --pretty=format:"%h - %an: %s"'
    }
  }
    stage('Stage 1: Build') {
      steps {
        echo 'Building the project with a build tool (e.g., Maven/Gradle/npm).'
        // sh 'mvn -B -DskipTests clean package'
      }
    }

    stage('Stage 2: Unit & Integration Tests') {
      steps {
        echo 'Running unit tests (e.g., JUnit) and integration tests.'
        // sh 'mvn -B test'                 // unit tests
        // sh 'mvn -B verify -Pintegration' // integration tests profile
      }
      post {
        always {
          echo 'Publishing test results (e.g., JUnit reports).'
          // junit 'target/surefire-reports/*.xml'
          // junit 'target/failsafe-reports/*.xml'
        }
      }
    }

    stage('Stage 3: Code Analysis') {
      steps {
        echo 'Static analysis with SonarQube/Checkstyle/PMD.'
        // withSonarQubeEnv('SonarQube') { sh 'mvn sonar:sonar' }
        // sh 'mvn checkstyle:check pmd:check'
      }
    }

    stage('Stage 4: Security Scan') {
      steps {
        echo 'Security scan for vulnerabilities (OWASP Dependency-Check/Snyk).'
        // sh 'mvn org.owasp:dependency-check-maven:check -Dformat=HTML -Dout=target'
        // sh 'snyk test'
      }
      post {
        always {
          echo '(Would publish security report artifacts here)'
          // archiveArtifacts artifacts: 'target/dependency-check-report.*', fingerprint: true
        }
      }
    }

    stage('Stage 5: Deploy to Staging') {
      steps {
        echo 'Deploying build artifact to STAGING (e.g., AWS EC2/Docker/K8s).'
        // sh '''
        //   docker build -t $APP_NAME:${BUILD_NUMBER} .
        //   docker tag $APP_NAME:${BUILD_NUMBER} myrepo/$APP_NAME:${BUILD_NUMBER}
        //   docker push myrepo/$APP_NAME:${BUILD_NUMBER}
        //   # kubectl set image deploy/$APP_NAME $APP_NAME=myrepo/$APP_NAME:${BUILD_NUMBER} --namespace=staging
        // '''
      }
    }

    stage('Stage 6: Integration Tests on Staging') {
      steps {
        echo "Running end‑to‑end/API/UI tests against ${STAGING_URL} (e.g., Postman/Selenium/Cypress)."
        // sh 'newman run tests/collection.json --env-var baseUrl=$STAGING_URL'
        // sh 'npm run cypress:run -- --config baseUrl=$STAGING_URL'
      }
    }

    stage('Stage 7: Deploy to Production') {
      when { branch 'main' }          // only promote from main
      steps {
        echo 'Promoting the release to PRODUCTION (e.g., AWS EC2/K8s).'
        // sh '''
        //   # kubectl set image deploy/$APP_NAME $APP_NAME=myrepo/$APP_NAME:${BUILD_NUMBER} --namespace=prod
        //   # or aws deploy push/start-deployment ...
        // '''
        echo "Production URL: ${PROD_URL}"
      }
    }
  }

  post {
    success { echo 'Pipeline completed successfully.' }
    failure { echo 'Pipeline failed — check stage logs.' }
  }
}
