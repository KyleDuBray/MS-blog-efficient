# MS-blog-efficient
A blog app example using Express and React. Demonstrates use of microservices architecture efficiently by using an event bus and query service to rid of unnecessary multitudes of requests. PLEASE NOTE: This is not a production-ready application. It is only meant to serve as an introductory example to microservices architecture.

## Basic Structure

The client uses React for the front end. Creating a post through front-end form submission will send a request to the "posts" service, which will emit an event to the event bus. The event bus will then reach out to all other services- comments, query, and moderation to communicate this event. The event bus keeps track of all events fired in a local data structure as well (in order to allow event syncing for the query service).
Creating a comment will also cause the event-bus to fire this event, which, along with any posts, will be sent to the query service to keep track of the posts and all subsequent comments. This allows for more efficient data retrieval for allowing the client to receive all post and comment data from ONLY the query service.
The moderation service handles the logic for determing which comments can stay and which ones are rejected (right now, it will reject any comment that contains the string "orange").

## Containerization and Continuous Deployment in Development

This project uses Dockerfiles to containerize each service (each directory, besides /infra, in the root can be considered one service in this case). The infra/k8s directory contains the configuration files for the pod deployments and the services that allow communication among the pods and for requests from the client.  The skaffold.yaml file contains the configuration to allow for all deployments to be started automatically. Before listing the necessary commands to get started, the next section will list all necessary dependencies.

## System Requirements

[Docker](https://www.docker.com/products/docker-desktop) is required for the image creation and container runtime.

[Kubernetes](https://kubernetes.io/docs/tasks/tools/) is required for the pod, deployment, and service creation, allowing for docker container orchestration.

[Skaffold](https://skaffold.dev/docs/install/) is required for the continous deployment during development.

[NGINX Ingress Controller for Kubernetes](https://kubernetes.github.io/ingress-nginx/deploy/) is required to create the custom ingress service in kubernetes, to control traffic to the pods.

In order to make the "posts.com" host work in the ingress configuration, you must change your OS routing rules to connect to the local machine instead of the real "posts.com."

For Windows:
in C:\Windows\System32\drivers\etc\hosts 
OR
For Mac/Linux:
in /etc/hosts add:
### `127.0.0.1 posts.com`

*If you are using minikube, run `minkkube ip` to determine the correct ip instead of 127.0.0.1.

## Scripts

If all requirements were installed correctly, and there are no conflicting port processes, you may now run `skaffold dev` in the root directory to automatically start all deployments and services with automatic restarting upon changes. You can exit the process with ctrl-c, however, in Windows, you may have to run `skaffold delete` to delete all deployments/services.

## Acknowledgements

This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).

This project is based off of [Stephen Grider's Microservices with Node JS and React Udemy course](https://www.udemy.com/course/microservices-with-node-js-and-react/).
