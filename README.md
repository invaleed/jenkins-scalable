# jenkins-scalable

```sh
# kubectl create -f jenkins-deployment.yaml 
# kubectl create -f jenkins-service.yaml 
# kubectl create -f jenkins-ingress.yaml 
```

Creates a service account
```sh
# kubectl -n default create sa jenkins
```

Gives cluster-admin permissions to the new account
```sh
# kubectl create clusterrolebinding jenkins --clusterrole cluster-admin --serviceaccount=default:jenkins
```

Retrieves the secret
```sh
# kubectl get -n default sa/jenkins --template='{{range .secrets}}{{ .name }} {{end}}' | xargs -n 1 kubectl -n default get secret --template='{{ if .data.token }}{{ .data.token }}{{end}}' | head -n 1 | base64 -d -
```

Copy the whole content printed at the console and go to Jenkins > Credentials > System > Global credentials > Add Credentials, change the Kind drop-down options to Secret text and past into Secret, create with name "jenkins-sa".

# Configure Jenkins
```sh
-- Kubernetes
Name : kubernetes
Kubernetes URL : https://34.69.72.28 << "kubectl cluster-info | grep master"
Credentials : jenkins-sa
Jenkins URL : http://10.28.2.12:8080/jenkins/ << "kubectl describe pod jenkins-xxx | grep IP:"

-- Pod Template
Name : jenkins-slave
Namespace : default
Labels : jenkins-slave

-- Container Template
Name : jenkins-slave
Docker image : jenkins/jnlp-slave

Lets the others to be default
```

# Create Job & Testing !!!
