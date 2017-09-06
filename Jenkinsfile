node{

    checkout scm
	def envPATH
    dir('BuildQuality'){
        stage('Preparation'){
                        
            git 'https://github.com/sanpatnaik/sampleprj.git'
            //mvnHome = tool 'Maven'
			envPATH = "${tool 'Ant'}/bin:${env.PATH}"
        }

        stage('Build') {
            // Run the maven build
            if (isUnix()) {
                sh "'${envPATH}/bin/ant Build'"
            } 
		//else {
               // bat(/"${envPATH}/bin/ant")
           // }
        }
        
        stage('SonarQube Analysis') { 
           // def mvnHome
            //mvnHome = tool 'Maven'
            withSonarQubeEnv('Sonar') { 
                if (isUnix()) {
                    sh "'${mvnHome}/bin/mvn' org.sonarsource.scanner.maven:sonar-maven-plugin:3.3.0.603:sonar -f pom.xml "+ 
                    " -Dsonar.projectKey=org.sonarqube:java-sonar-ANT " +
                    " -Dsonar.projectKey=org.sonarqube:java-sonar-ANT " +
                    " -Dsonar.projectName='Java :: Simple Spring Project-ANT' " +
                    " -Dsonar.projectVersion=1.0 " +
                    " -Dsonar.language=java " +
                    " -Dsonar.sources=. " +
                    " -Dsonar.tests=. " +
                    " -Dsonar.test.inclusions='**/*Test*/**' " +
                    " -Dsonar.exclusions='**/*Test*/**' "
                } else {
                    bat (/"${mvnHome}\bin\mvn" org.sonarsource.scanner.maven:sonar-maven-plugin:3.3.0.603:sonar -f pom.xml -Dsonar.projectKey=org.sonarqube:java-sonar-ANT -Dsonar.projectName="Java :: Simple Spring Project-ANT" /)
                }    
            }        
        }

        //stage('Unit Test Results') {
           // junit '**/target/surefire-reports/TEST-*.xml'
            //archive 'target/*.jar'
        //}

    }
}

stage name:'Deploy to staging', concurrency:1
    node {
                //sh 'sudo docker run -d -p=3000:80 --network=bundlev2_prodnetwork nginx'
        dir('BuildQuality'){
        sh 'sudo docker-compose up -d --build'
    }
                
}
stage name:'Shutdown staging'
    node {
                
        dir('BuildQuality'){
        sh 'sudo docker-compose stop'
    }
                
}
        

