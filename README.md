# ci-cd-pipeline-avec-jenkins-sur-gcp
Pipeline CI/CD avec Jenkins et Webhooks GitHub sur Google Cloud Platform (GCP)


Introduction

Ce projet configure un pipeline CI/CD avec Jenkins sur Google Cloud Platform (GCP) et utilise des Webhooks GitHub pour automatiser l'intégration et la livraison continues. Le pipeline se déclenche automatiquement lors de chaque commit sur le dépôt GitHub pour construire, tester et déployer le code.



Pré-requis

Compte GitHub
Compte Google Cloud Platform (GCP)
Accès à une machine virtuelle Ubuntu sur GCP
Jenkins installé sur la machine virtuelle



Configuration du Projet GitHub

Créer un Dépôt GitHub :
Nom du dépôt : ci-cd-pipeline-avec-jenkins-sur-gcp

Cloner le Dépôt :
git clone https://github.com/Serchgalarza/ci-cd-pipeline-avec-jenkins-sur-gcp.git
cd ci-cd-pipeline-avec-jenkins-sur-gcp

Créer un Jenkinsfile :
Créez un fichier nommé Jenkinsfile avec le contenu suivant :

pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
            }
        }
    }
}

Commit et Push :
Pour ajouter le Jenkinsfile et le pousser vers le dépôt, utilisez :
git add Jenkinsfile
git commit -m "Ajouter le Jenkinsfile pour configurer le pipeline CI/CD"
git push origin main



Déploiement de Jenkins sur GCP

Créer une Instance GCP :
Connectez-vous à Google Cloud Console.
Accédez à "Compute Engine" -> "VM instances".
Créez une nouvelle instance de type micro avec Ubuntu comme système d'exploitation.

Configurer les Règles de Pare-feu :
Autorisez le trafic sur le port 8080 pour accéder à Jenkins (en cochant "Allow HTTP traffic", "Allow HTTPS traffic", et en ajoutant une règle pour le port 8080 si nécessaire).

Installer Jenkins :
Connectez-vous à la VM via SSH et suivez les instructions suivantes :
sudo apt update
sudo apt upgrade -y
sudo apt install openjdk-11-jdk -y
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary > /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt install jenkins -y
sudo systemctl start jenkins
sudo systemctl enable jenkins

Accéder à Jenkins :
Ouvrez un navigateur et accédez à l'URL suivante, en remplaçant <external-ip> par l'adresse IP de votre instance GCP :
http://<external-ip>:8080




Configuration de Jenkins

Créer un Nouveau Job :
Connectez-vous à Jenkins via l'URL mentionnée ci-dessus.
Cliquez sur "New Item", entrez le nom du projet ci-cd-pipeline-avec-jenkins-sur-gcp, sélectionnez "Pipeline" et cliquez sur "OK".

Configurer le Job :
Description : Ajoutez une description si nécessaire.
Build Triggers : Sélectionnez "GitHub hook trigger for GITScm polling".
Pipeline : Choisissez "Pipeline script from SCM".
SCM : Sélectionnez "Git".
URL du Repository : https://github.com/Serchgalarza/ci-cd-pipeline-avec-jenkins-sur-gcp.git
Branch : Utilisez la branche feature/ajouter-jenkinsfile (ou main si c'est la branche principale).
Cliquez sur "Save" pour enregistrer le job.



Configuration des Webhooks GitHub

Ajouter un Webhook :
Accédez à votre dépôt GitHub : https://github.com/Serchgalarza/ci-cd-pipeline-avec-jenkins-sur-gcp.
Allez dans "Settings" -> "Webhooks".
Cliquez sur "Add webhook".
Payload URL : http://<external-ip>:8080/github-webhook/
Content type : application/json
Which events would you like to trigger this webhook? : Sélectionnez "Just the push event".
Cliquez sur "Add webhook" pour enregistrer le webhook.



Vérification et Dépannage

Vérification : Effectuez un commit dans le dépôt GitHub et vérifiez si le pipeline Jenkins se déclenche automatiquement.

Dépannage : Consultez les logs de Jenkins pour résoudre les erreurs potentielles et assurez-vous que les Webhooks sont correctement configurés.


