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

# Configure jenkins

# Create Job & Testing!!!
