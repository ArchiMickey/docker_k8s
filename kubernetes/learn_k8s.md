#Kubernetes Learning Sheet

##Basic Command
1. Open dashboard with minikube:
`minikube dashboard --url`

2. Create deployment:
`kubectl create deployment hello-node --image=k8s.gcr.io/echoserver:1.4`

3. View services:
`kubectl get services`

4. View deployments:
`kubectl get deployments`

5. View pods:
`kubectl get pods`

6. `kubectl apply -f {YAML_PATH}`: create object from yaml file

###Create Service:
1. Expose the pod with `kubectl expose`:
`kubectl expose deployment hello-node --type=LoadBalancer --port=8080`

    --type=LoadBalancer
    : expose your Service outside of the cluster so clients from different port can connect

2. View the Service:
`kubectl get services`

3. Run the service with Minikube:
`minikube service hello-node`

###Clean Up
* Stop the cluster
`kubectl delete service hello-node`
`kubectl delete deployment hello-node`

* Stop the Minikube VM
`minikube stop`

* Delete the Minikube VM
`minikube delete`

##Accessing apps
* Nodeport
  : opens a specific port, and any traffic that is sent to this port is forwarded to the service
* LoadBalancer
  : each service gets its own IP address

  When using LoadBalancer, `minikube tunnel` should be used

  Run the `tunnel` and expose a service so the service can be reached by address

##Deployment
https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
`.metadata.name`: name of the deployment
`.spec.replicas`: num of replicated pods
`.spec.selector`: which pods to manage

* Create deployment from ymal file:
`kubectl apply -f https://k8s.io/examples/controllers/nginx-deployment.yaml`

* See the rollout status:
`kubectl rollout status deployment/nginx-deployment`

* See the ReplicaSet:
`kubectl get rs`

* Get details of deployment:
`kubectl describe deployments`

* Roll back a deployment
`kubectl set image deployment/nginx-deployment nginx=nginx:1.161 `
Set the image name as nginx:1.161

* Roll back to previous revision:
`kubectl rollout undo deployment/nginx-deployment`

* Scale a deployment
`kubectl scale deployment/nginx-deployment --replicas=10`