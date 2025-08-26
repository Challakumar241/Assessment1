**Pipeline for Tomact**

pipeline {

&nbsp;   agent any



&nbsp;   stages {

&nbsp;       stage("Checkout Code") {

&nbsp;           steps {

&nbsp;               git 'https://github.com/Challakumar241/addressbook-cicd-project'

&nbsp;               echo "Cloning code"

&nbsp;           }

&nbsp;       }



&nbsp;       stage("Compile") {

&nbsp;           steps {

&nbsp;               sh 'mvn -B -e compile'

&nbsp;           }

&nbsp;       }



&nbsp;       stage("Test") {

&nbsp;           steps {

&nbsp;               sh 'mvn -B test'

&nbsp;           }

&nbsp;       }



&nbsp;       stage("Package") {

&nbsp;           steps {

&nbsp;               sh 'mvn -B package'

&nbsp;           }

&nbsp;       }



&nbsp;       stage("Deploy") {

&nbsp;           steps {

&nbsp;               sh '''

&nbsp;                   sudo cp /var/lib/jenkins/workspace/project-pipeline/target/addressbook.war \\

&nbsp;                   /home/ubuntu/apache-tomcat-9.0.108/webapps

&nbsp;               '''

&nbsp;           }

&nbsp;       }

&nbsp;   }

}









