pipeline {
  agent any 
  tools {
    maven 'Maven'
  }
  stages {
    stage ('Initialize') {
      steps {
        sh '''
                echo "PATH = ${PATH}"
                echo "M2_HOME = ${M2_HOME}"
            ''' 
      }
    }
    
    stage ('Check secrets in repository') {
      steps {
      sh 'cd /dumpster_diver/DumpsterDiver/ ; sudo python3 DumpsterDiver.py -p ${WORKSPACE} -o DumpsterDiver_output'
      sh 'trufflehog3 https://github.com/Suyashk96/webGoat_java.git -f html -o truffelhog_output || true'
      }
    }
    
    stage ('Install dependencies') {
      steps {
        sh 'mvn clean install -DskipTests'
      }
    }
    
    stage ('OWASP Dependency-Check Vulnerabilities') {
            steps {
                dependencyCheck additionalArguments: ''' 
                    -o "./" 
                    -s "./"
                    -f "ALL" 
                    --prettyPrint''', odcInstallation: 'OWASP-DC'

                dependencyCheckPublisher pattern: 'dependency-check-report.xml'
            }
        }   
   }  
}
