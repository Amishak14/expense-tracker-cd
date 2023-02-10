pipeline {
    agent any
    stages {
        stage("Checkout") {
            steps {

               checkout scm
        }


    }

        stage("update git"){
        steps{
            withCredentials([usernamePassword(credentialsId: 'my-git', passwordVariable: 'pass', usernameVariable: 'user')]) {
                  script {
                        env.encodedPass=URLEncoder.encode(pass, "UTF-8")
                    }

 

        sh "git config user.email 18ec007.amisha@sjec.ac.in"
        sh "git config user.name Amishak14"
        sh "cat ./my-folder/manifest.yaml"
        echo "${DOCKERTAG}"
        sh "sed -i 's+image-registry.openshift-image-registry.svc:5000/amisha-jenkins/node-backend:.*+image-registry.openshift-image-registry.svc:5000/amisha-jenkins/node-backend:${DOCKERTAG}+g' manifest.yaml"
        sh "sed -i 's+image-registry.openshift-image-registry.svc:5000/amisha-jenkins/node-frontend:.*+image-registry.openshift-image-registry.svc:5000/amisha-jenkins/node-frontend:${DOCKERTAG}+g' manifest.yaml"
        sh "cat manifest.yaml"
        sh "git add ."
        sh "git commit -m 'done by jenkins job expense-tracker-cd-pipeline' "
       // sh "git pull"
        sh 'git push https://$user:$encodedPass@github.com/$user/expense-tracker-cd.git HEAD:branch'
            }
        }
        }
        
        
         stage("deploy the application") {
        steps {
            script {
                openshift.withCluster() {
                    openshift.withProject("$PROJECT_NAME") {
                        echo "Using project: ${openshift.project()}"
                         sh 'oc project "$PROJECT_NAME"'
                         sh 'oc apply -f manifest.yaml'
                    }
                 }
            }
        } 
    }  
        }
}
