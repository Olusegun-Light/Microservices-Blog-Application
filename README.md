# Microservices Blog Application

## Overview

This project is a microservices-based blog application built using React for the client-side and Express for the server-side. The application allows users to create posts and comments. Comments are moderated to check for specific content before being displayed.

## Architecture

The application is divided into several microservices:
1. **Posts Service**: Manages post creation.
2. **Comments Service**: Manages comment creation and moderation status.
3. **Query Service**: Aggregates data from posts and comments for the client.
4. **Moderation Service**: Checks comments for prohibited content.
5. **Event Bus**: Manages the communication between all services by forwarding events.

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

1. Start the posts service:
    ```bash
    cd posts
    npm start
    ```

2. Start the comments service:
    ```bash
    cd comments
    npm start

    ```

3. Start the query service:
    ```bash
    cd event-bus/query
    npm start

    ```

4. Start the moderation service:
    ```bash
    cd moderation
    npm start

    ```

5. Start the event bus:
    ```bash
    cd event-bus
    npm start
    ```

### Running the Client

1. Start the React client:
    ```bash
    cd client
    npm start
    ```

2. Open your browser and navigate to `http://localhost:3000`.

