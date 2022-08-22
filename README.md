# Deploying a containerized web application | Kubernetes Engine

In this Project, I will show you how to deploy the web application as a load-balanced set of replicas that can scale to the needs of your users.
In my previous project, I package a web application in a Docker container image and run the Container image.
If You want to know visit my Project [Deploy-WebApp-With-Docker](https://github.com/souravsk/Deploy-WebApp-With-Docker)

### Step:- 1

- First we are going to push our docker image into [Docker Hub](https://hub.docker.com/repositories)
- Here are the steps that worked for me :
1. Login to the docker.

```
docker login -u sovu

```

2. Tag your image build

my image name here is : **mylocalimage** and by default it has tag : **latest**and my username is : **sovu** as registered with docker cloud, and I created a public repository named : **dockerhub**

so my personal repository becomes now : **sovu/dockerhub** and I want to push my image with tag : `myfirstimagepush`

I tagged as below :

```
docker tag mylocalimage:latest sovu/dockerhub:myfirstimagepush

```

3. Pushed the image to my personal docker repository as below

```
docker push sovu/dockerhub:myfirstimagepush

```

### Step:- 2

- Now we will create a file with the name `deployment.yml` and then write the code for it.

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web
spec:
  replicas: 1
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
      - name: portfolio-website
        image: docker.io/sovu/portfolio-website:latest
        ports:
        - containerPort: 8080

```

> If You are not that familiar with the YAML let me tell you that indentation is very important if the indentation will be wrong it will not work.
> 

Now save the file

### Step:- 3

- Now I'm using [Minikube](https://minikube.sigs.k8s.io/docs/start/) as my Kubernetes cluster to run my web application
- Start the minikube

```
minikube start

```

- it will start one control node and one worker node.

### Step:- 4

- Now let's run the command to deploy our web application.

```
kubectl apply -f deployment.yml

```

- `kubectl` Kubernetes command-line tool, kubectl, allows you to run commands against Kubernetes clusters. You can use kubectl to deploy applications, inspect and manage cluster resources, and view logs
- `apply` Manages applications through files defining Kubernetes resources. It creates and updates resources in a cluster through running `kubectl apply`
- `f` it mean we are applying file.
- We can use a command to see the deployment

```
kubectl rollout status deployment/web

```

### Step:- 5

- Now we have successfully deployed our containerized web page or docker image that was pushed in the docker hub repository.
- Now we can expose the port so that we can see that we have deployed the application.

```
kubectl port-forward deployment/web-app 8080:80

```

- We are exposing our web application at host port 8080.

### Step:- 6

- Now go to the browser and type at URL address bar `localhost:8080` and your web page is working.
