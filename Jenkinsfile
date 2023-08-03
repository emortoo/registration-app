pipeline{
  agent any 
  tools{
    maven "maven3.9.3"  // name should match the name of maven plugin in jenkens
  }
  stages{
    stage('Git Clone'){
      steps{
        echo "cloning the lastest applications version"
        git "https://github.com/emortoo/registration-app"
      }
    }
      stage('Maven Build'){
      steps{
        sh "echo Running unitTesting"
        sh "mvn clean package"
        echo "echo test successful and backupArtifacts created"
      }
    }
      stage('Sonar Code Quality'){
      steps{
        sh "echo Running static code analysis"
        sh "mvn sonar:sonar"
        sh "echo All conditions passed"
      }
    }
      stage(' Nexus upload Artifacts'){
      steps{
        sh "echo Preparing artifacts for upload"
        sh "mvn deploy"
        sh "echo backing up Artifacts in nexus"
      }
    }
      stage('Docke Predeployment'){
      steps{
        sh "echo creating docker image"
        sh "docker build -t emortoo/registration-app . "
        sh "docker push emortoo/registration-app"
      }
    }
    stage('UnDeploy'){
      steps{
        sh "echo UNDEPLOYING existing application"
        sh "docker rm -f webapp"
      }
    }
    stage('deployment'){
      steps{
        sh "echo application ready for deployment"
        sh "docker run -d -p 8000:8080 --name webapp mylandmarktech/maven-web-app"
      }
    }
  stage('emailNotification'){
    steps{
      sh "echo deployment successful"
    }
  }

  }
}