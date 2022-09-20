[Medium link](https://medium.com/geekculture/how-to-deploy-spring-boot-and-mongodb-to-kubernetes-minikube-71c92c273d5e)

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
    - In your browser go to: [localhost:8080](localhost:8080) or [127.0.0.1:8080](127.0.0.1:8080)


