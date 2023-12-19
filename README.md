# Kubernetes Dummy Application Project

This project is designed to demonstrate the deployment of a dummy application using Kubernetes. It consists of multiple microservices including Auth API, User API, Tasks API, and a Frontend. The project allows for creating, managing authorization tokens, handling user tasks, and serves a frontend for user interaction. Please note that the code for the app, including the JavaScript, HTML, JSON and CSS is not authored by the repository owner.

## Prerequisites

To run this project, ensure the following prerequisites are met:

- Docker installed on your machine
- Docker Hub account to store your Docker images
- kubectl installed locally to interact with Kubernetes clusters
- Minikube installed for local Kubernetes development


## Getting Started

1. Clone the Repository:

    ```bash
    git clone https://github.com/Enoch-Gonzalez/06-kubernetes-multiples-microservies-local-depl.git
    ```

    ```bash
    cd /06-kubernetes-multiples-microservies-local-depl
    ```

## Setting Up the Project

1. Create a Docker Hub repository for your images.

2. Build Docker images for each service and tag them with your Docker Hub repository.

3. Building Docker Images.

- Navigate into each service directory and create Docker images:

    + **Auth API**

        ```bash
        cd auth-api
        ```

        ```bash
        docker build -t <DOCKERHUB_USERNAME>/auth-api .
        ```

        ```bash
        docker push <DOCKERHUB_USERNAME>/auth-api
        ```

    + **Frontend API**

        ```bash
        cd ../frontend
        ```

        ```bash
        docker build -t <DOCKERHUB_USERNAME>/frontend .
        ```

        ```bash
        docker push <DOCKERHUB_USERNAME>/frontend
        ```

    + **Tasks API**

        ```bash
        cd ../tasks-api
        ```

        ```bash
        docker build -t <DOCKERHUB_USERNAME>/tasks-api .
        ```

        ```bash
        docker push <DOCKERHUB_USERNAME>/tasks-api
        ```

    + **Users API**

        ```bash
        cd ../users-api
        ```
        
        ```bash
        docker build -t <DOCKERHUB_USERNAME>/users-api .
        ```

        ```bash 
        docker push <DOCKERHUB_USERNAME>/users-api
        ```

    Push the images to your Docker Hub repository.

    These commands provide a clear pathway for navigating into each service directory, building Docker images, and pushing them to your Docker Hub repository. Adjust `<DOCKERHUB_USERNAME>` to your actual Docker Hub username.

4. Check Minikube Status:

- Before deploying, ensure Minikube is up and running:

    ```bash
    minikube status
    ```

- If Minikube is not running, start it using the configured driver:

    ```bash
    minikube start --driver=<your-drivert-set-when-installing-minikube-docker-or-virtualvm>
    ```

5. Deploy Kubernetes Objects.

- Before applying the YAML files, ensure to update them with your specific repository and image names.

- Navigate to each `deployment.yaml` file in the `kubernetes` directory. Look for the `spec -> containers -> image` field in each file and replace `<DOCKERHUB_USERNAME>/<IMAGE_NAME>` placeholders with your Docker Hub username and chosen image names.

- Here's an example snippet of the YAML file where you should make the changes:

    ```yaml
    spec:
        containers:
            - name: auth
              image: <DOCKERHUB_USERNAME>/auth-api:latest # Replace this line with your Docker Hub username and image name
    ```


6. Apply the YAML files to create Kubernetes objects:

    ```bash
    kubectl apply -f kubernetes/auth-deployment.yaml
    kubectl apply -f kubernetes/auth-service.yaml
    ```

    ```bash
    kubectl apply -f kubernetes/frontend-deployment.yaml
    kubectl apply -f kubernetes/frontend-service.yaml
    ```

    ```bash
    kubectl apply -f kubernetes/tasks-deployment.yaml
    kubectl apply -f kubernetes/tasks-service.yaml
    ```

    ```bash
    kubectl apply -f kubernetes/users-deployment.yaml
    kubectl apply -f kubernetes/users-service.yaml
    ```

7. Check Service Status

    ```bash
    kubectl get service frontend-service
    ```

- The external IP might show as "Pending" because Minikube doesn't provide an external IP for services of type LoadBalancer. For cloud providers like AWS, an external IP would be displayed here.

8. Access the Application in Minikube:

    ```bash
    minikube service frontend-serivce
    ```

9. Signup to the app:

- To sign up to the app send a post request to /signup:

http://<minikube-serivce-frrtonend-service>/signup

10. Signin to the app:

- Once you receive a message that the user was successfully created sent a request to /login:

http://<minikube-serivce-frrtonend-service>/login

11. Add tasks to your app.

## Cleaning Up

1. To delete the resources created by the YAML file, use the following command:

    ```bash
    kubectl delete -f kubernetes/auth-deployment.yaml
    kubectl delete -f kubernetes/auth-service.yaml
    ```

    ```bash
    kubectl delete -f kubernetes/frontend-deployment.yaml
    kubectl delete -f kubernetes/frontend-service.yaml
    ```

    ```bash
    kubectl delete -f kubernetes/tasks-deployment.yaml
    kubectl delete -f kubernetes/tasks-service.yaml
    ```

    ```bash
    kubectl delete -f kubernetes/users-deployment.yaml
    kubectl delete -f kubernetes/users-service.yaml
    ```

- This will remove the service, deployment, persistent volume claim, and the persistent volume from your Kubernetes cluster.

2. Stop Minikube:

    ```bash
    minikube stop
    ```

3. Delete Minikube:

    ```bash
    minikube delete
    ```

4. To remove docker Containers if docker driver was used to start Minikube:

    ```bash
    docker rm $(docker ps -aq)
    ```

5. To remove docker images if docker driver was used to start Minikube:

    ```bash
    docker images -q | xargs docker rmi
    ```

- The above commands will stop and delete Minikube and then remove all Docker containers and images on your system. Be cautious when using the docker rm and docker rmi commands, as they will remove all containers and images, not just those related to Minikube.

### Note

This project demonstrates setting up a network for communication between multiple pods. Feel free to explore and modify as needed.

## License

This project is licensed under the MIT License - see the LICENSE file for details.