### CD - Deploy to LKE cluster from Jenkins Pipeline

### Technologies Used:

Kubernetes, Jenkins, Linode LKE, Docker, Linux

### Project Description:

1- Create K8s cluster on LKE

2- Install kubernetes ctl as Jenkins Plugin

3- Adjust Jenkinsfile to use Plugin and deploy to LKE cluster

### Instructions

###### Step 1: Create k8s cluster on LKE

###### Step 2: Download the kubeconfig.yaml from k8s cluster on LKE

###### Step 3: export kuberconfig.yaml file as a env variable:KUBECONFIG

![image](images/Screenshot%202023-03-14%20at%2012.57.16%20am.png)

###### Step 4: Create linode credentials by kubeconfig.yaml file on jenkins

![image](images/Screenshot%202023-03-14%20at%2012.58.13%20am.png)

###### Step 5: Install kubernetes CLI plugin on Jenkins

![image](images/Screenshot%202023-03-14%20at%201.09.36%20am.png)

###### Step 7: Update the jenkinsfile to authenticate kubectl can communicate with k8s cluster on Linode

```
#!/usr/bin/env groovy

pipeline {
    agent any
    stages {
        stage('build app') {
            steps {
               script {
                   echo "building the application..."
               }
            }
        }
        stage('build image') {
            steps {
                script {
                    echo "building the docker image..."
                }
            }
        }
        stage('deploy') {
            steps {
                script {
                   echo 'deploying docker image...'
                   withKubeConfig([credentialsId: 'lke-credentials', serverUrl: 'https://79fa9228-1d11-47ec-870b-33106d53122b.eu-central-2.linodelke.net']) {
                       sh 'kubectl create deployment nginx-deployment --image=nginx'
                   }
                }
            }
        }
    }
}
```
