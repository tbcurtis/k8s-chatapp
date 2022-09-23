- [Deploying SpringBoot and MongoDB link](https://medium.com/geekculture/how-to-deploy-spring-boot-and-mongodb-to-kubernetes-minikube-71c92c273d5e)
- [Grafana Medium Link Setup](https://medium.com/codex/setup-grafana-loki-on-local-k8s-cluster-minikube-90450e9896a8)
## Build and create MongoDB:
- In your terminal change to the MongoDB folder: `cd ChatApp/MongoDB`
- Run `kubectl apply -f .`
    - This will create the mongoDB deployment, secret and service.

## Build and run chat app:
- In your terminal change to the Spring Chat App folder: `cd ChatApp/Spring-Websocket-Chat-Application-with-MongoDB`
- Then run: `maven clean install`
    - This creates the chatapp-0.0.1-SNAPSHOT.jar file under Spring-Websocket-Chat-Application-with-MongoDB/target
- Next, run: `docker build -t {docker_username}/chatapp .`
    - This creates the chatapp docker image
    - If you wish to push this image to docker hub, run: `docker push {link}`
- Now that we have created the docker image for the chat app, we can move on to deploying it
    - Change to the ChatApp directory `cd ../`
    - Run `kubectl apply -f .`
        - This will create the configmap, deployment, and service in K8S for the chat app

## How to get the ChatApp working locally:
- After the ChatApp LoadBalancer service is created:
    - Run: `minikube tunnel`
        - This creates a tunnel between the minikube kubernetes cluster and your local machine, **you must leave the terminal window open for this to work**
        - You can see the external IP being created by this when you run `kubectl get services`
    - In your browser go to: [localhost:8080](http://localhost:8080) or [127.0.0.1:8080](http://127.0.0.1:8080)


### Grafana Setup
- To port forward for grafana, use this command: `kubectl port-forward service/grafana 80:80 --namespace=monitoring`
- Then go to [localhost:80](http://localhost:80)

### From the [Google Doc](https://docs.google.com/document/d/e/2PACX-1vQ3LqEaLLo8guqESJ9ktIyk7vTDJWacQnBCDJvwreLEgZGi6Q-_ujMCOq5ypbyYi0--o-87QQvivVYl/pub)
The Goals of the assignment are to demonstrate:

1) A working knowledge of kubernetes deployments.
2) A understanding of services and networking within kubernetes
3) How to build applications that take advantage of kubernetes features like secrets and configmaps.
4) How to use 12 Factor App methodology to build services that are able to leverage the power of kubernetes.

To complete the assignment you will need to build some portions of a **prototype** application in the language of your group’s choice.  This language may change from portion to portion, as the group sees fit, as this leverages the separation provided by kubernetes and demonstrates that abstraction of services in relation to each other in a container environment.

The application will be a small chat service, which allows multiple clients to chat over websockets, while logging the timestamp, user, and message to a database.  The output logs of all involved services within kubernetes should be transported to a logging solution, which may be any you select.  This includes but is not limited to various pre-packaged logging services, or even a hand-built service that runs alongside the primary service in it’s pod.

All services should provide health checks to kubernetes.

The suggested structure is:
* A log database pod
* A websocket server pod
* A message database pod

The websocket server pod should be able to replicate up to 3 pods, with a minimum of one.

It is highly recommended that you make use of “kubectl -k apply” as a means of deploying multiple files to your k8s instance. For more information: https://kubernetes.io/docs/tasks/manage-kubernetes-objects/kustomization/

Please remember in all cases that this is a **prototype** level application, and while it should conform to 12 factor it need not be robust, or even well written.  The point is purely to exercise kubernetes and it’s usage from a code-first standpoint.

**Why websockets?** Websockets are chosen as they are a well supported technology in many languages, if your language does not have good library support for websockets, then please feel free to use alternative means such as long-polling or forced refreshes for your frontend instead.
