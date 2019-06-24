# SampleAmbassador
This Project builds a sample app with tha Ambassador API gate way. It runs locally on minikube.


## STEPS TO DEPLOY AMBASSADOR WITH CONTAINER IMAGES

1. Clone this repository, extract and go into the extracted folder.
2. Run minikube in powershell.
3. Run the below commands:

## 1. Deploy Ambassador:
Deploy ambassador with RBAC/NO-RBAC as per requirements. 

On minikube: 

If RBAC enabled:

`$ kubectl apply -f https://getambassador.io/yaml/ambassador/ambassador-rbac.yaml `

else if RBAC NOT enabled:

`$ kubectl apply -f https://getambassador.io/yaml/ambassador/ambassador-no-rbac.yaml`
 
## 2. Defining the Ambassador Service:

Ambassador is deployed as a Kubernetes Service that references the ambassador Deployment you deployed previously.
Make an ambassador-service.yaml file, Deploy it with Kubectl.

The YAML above creates a Kubernetes service for Ambassador of type LoadBalancer and configures the externalTrafficPolicy to propagate the original source IP of the client. All HTTP traffic will be evaluated against the routing rules you create. Note that if you're not deploying in an environment where LoadBalancer is a supported type (such as minikube), we'll need to change this to a different type of service, e.g., NodePort.

## 3. Create a service:

With images link and apply the service with 
`$ kubectl apply -f [service_name].yaml`
 
When the service is deployed, Ambassador will notice the getambassador.io/config annotation on the service, and use the Mapping contained in it to configure the route. (There's no restriction on what kinds of Ambassador configuration can go into the annotation, but it's important to note that Ambassador only looks at annotations on Kubernetes Services ( " | ").)
 
## 4. Testing the mapping:

`$ kubectl get svc -o wide ambassador`

If we are using minikube on our local machine, we'll be getting EXTERNAL-IP as` <NONE>/<PENDING>`, 

We can use: 

 `$  minikube service list`
 
It lists all the service like ambassador and ambassador admin, with the URL, we can then open the URL.
 
## 5. Diagnostics Service in Kubernetes:

Ambassador includes an integrated diagnostics service to help with troubleshooting. By default, this is not exposed to the Internet. 

To view it, we'll need to get the name of one of the Ambassador pods:

`$ kubectl get pods`

And then forwarding local port 8877(here) to one of the pods as:

`$ kubectl port-forward [pod-name] 8877`

