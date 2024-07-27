# Microservices Blog Application

## Project Description

This project is a full-featured blog application built using a microservices architecture. The application enables users to create posts and comments, with a moderation system to filter inappropriate content. By breaking down the application into multiple services, it demonstrates how to create a scalable and maintainable system using modern development practices.

### Key Features

- **Post Creation**: Users can create new blog posts with a title.
- **Commenting System**: Users can comment on posts, with each comment going through a moderation process.
- **Moderation**: Comments are automatically reviewed and flagged if they contain specific prohibited content.
- **Real-Time Updates**: The application updates in real-time, reflecting the current state of posts and comments.

### Technology Stack

- **Frontend**: Built with React, the user interface provides a seamless experience for creating posts and comments.
- **Backend**: Consists of multiple Node.js services using Express to handle different aspects of the application.
- **Event Bus**: Facilitates communication between services by broadcasting events, ensuring that all services stay in sync.
- **Data Storage**: Each service uses in-memory storage for simplicity, though it can be extended to use persistent storage solutions.


### Microservices Overview

1. **Posts Service**
   - Manages the creation and retrieval of posts.
   - Emits events to notify other services when a new post is created.

2. **Comments Service**
   - Handles the creation of comments for specific posts.
   - Emits events when new comments are created and when their status changes after moderation.

3. **Query Service**
   - Aggregates data from the Posts and Comments services to provide a unified view for the client.
   - Listens for events to maintain up-to-date data.

4. **Moderation Service**
   - Automatically reviews new comments for inappropriate content.
   - Changes the status of comments based on the moderation rules and emits events to update other services.

5. **Event Bus**
   - Centralized event management service that ensures all other services are kept up-to-date by broadcasting events they need to process.

### Kubernetes Configuration

The application is containerized and deployed using Kubernetes. The configuration files for the deployments and services are located in the `infra/k8s` directory. Here’s an overview of the configuration files:

- **Client Deployment and Service**: Defines the deployment and service for the frontend client application.
- **Comments Deployment and Service**: Defines the deployment and service for the comments service.
- **Event Bus Deployment and Service**: Defines the deployment and service for the event bus service.
- **Ingress Service**: Configures the NGINX ingress controller to route traffic to the appropriate services based on the URL path.
- **Moderation Deployment and Service**: Defines the deployment and service for the moderation service.
- **Posts Deployment and Service**: Defines the deployment and service for the posts service.
- **Query Deployment and Service**: Defines the deployment and service for the query service.

### Skaffold Configuration

The `skaffold.yaml` file orchestrates the building and deployment of the application using Skaffold. It includes:

- **Build Configuration**: Specifies how to build the Docker images for each service.
- **Deploy Configuration**: Specifies the Kubernetes manifests to be used for deploying the application.


## Services and Code Details

### Client

The client is built using React and includes several key components:

- **App Component**: The main component that renders the `PostCreate` and `PostList` components. It serves as the entry point for the application, providing the structure and basic layout.

- **PostCreate Component**: A form that allows users to create new posts. When the form is submitted, it sends a POST request to the posts service to create a new post.

- **PostList Component**: Fetches and displays a list of posts. Each post also includes a list of comments and a form to create new comments (`CommentCreate`). The component retrieves the posts data from the query service.

- **CommentCreate Component**: A form that allows users to create new comments on a specific post. When the form is submitted, it sends a POST request to the comments service to create a new comment.

- **CommentList Component**: Displays a list of comments for a specific post. Each comment is displayed based on its moderation status, showing appropriate messages for pending or rejected comments.

### Server

#### Posts Service

- **Posts Service**: 
  - Manages the creation of posts.
  - Stores posts in an in-memory object.
  - Listens for incoming HTTP POST requests to create new posts.
  - Emits `PostCreated` events to the event bus.

#### Comments Service

- **Comments Service**: 
  - Manages the creation and storage of comments for each post.
  - Stores comments in an in-memory object, indexed by post ID.
  - Listens for incoming HTTP POST requests to create new comments.
  - Emits `CommentCreated` events to the event bus.
  - Listens for `CommentModerated` events to update comment statuses.

#### Query Service

- **Query Service**: 
  - Aggregates posts and comments for easy retrieval by the client.
  - Stores data in an in-memory object.
  - Listens for `PostCreated`, `CommentCreated`, and `CommentUpdated` events to maintain an updated view of the data.
  - Provides an API endpoint to retrieve all posts along with their comments.

#### Moderation Service

- **Moderation Service**: 
  - Listens for `CommentCreated` events.
  - Moderates comments based on their content (e.g., rejects comments containing specific words).
  - Emits `CommentModerated` events to the event bus with the moderation status.

#### Event Bus

- **Event Bus**: 
  - Manages the communication between services by forwarding events to all services.
  - Stores all events in an in-memory array.
  - Provides an API endpoint to retrieve all events for synchronization purposes.

## Getting Started

### Prerequisites

- Node.js
- npm

### Installation

1. Clone the repository:
    ```bash
    git clone https://github.com/Olusegun-Light/Microservices-Blog-Application.git
    ```

2. Install dependencies for each service and the client:
    ```bash
    cd <service-directory>
    npm install
    ```

### Running the Services

  ```bash
    skaffold dev
  ```

### Conclusion

This project serves as an example of building a microservices-based application using modern technologies and Kubernetes for orchestration. It provides a scalable and maintainable solution for a simple blog application, illustrating the benefits of a microservices architecture.