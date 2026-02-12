# Get the login token
````shell
kubectl get secret headlamp-admin -n headlamp-app -o jsonpath='{.data.token}' | base64 -d
````
