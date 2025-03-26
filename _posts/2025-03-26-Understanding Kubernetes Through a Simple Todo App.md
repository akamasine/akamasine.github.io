---
title:  "Understanding Kubernetes Through a Simple Todo App"
layout: post
categories: Kubernetes
description: "How to deploy a simple Todo application on Kubernetes focusing on understanding Kubernetes components and how they work together to run the application."

keywords: "kubernetes,minikube,3tire,three-tire,deployment"
canonical_url: "https://blogs.gautamnischal.com.np/todo-application-on-kubernetes/"
author: "Nischal Gautam
---

## Introduction
In this guide, we'll explore how to deploy a simple Todo application on Kubernetes. We'll focus on understanding Kubernetes components and how they work together to run our application.

## Source Code
All code for this tutorial is available on GitHub:
```bash
git clone https://github.com/akamasine/Demo-Todo.git
```
![kubernetes.png](/assets/images/kubernetes.png)


## What We're Building
A 3-tier Todo application consisting of:
- Frontend: React application
- Backend: Node.js/Express API
- Database: PostgreSQL

## Understanding Kubernetes Components

### 1. Database Layer
Our PostgreSQL database is managed through:
- **ConfigMap**: Stores database configuration
- **Secret**: Securely stores database password
- **PersistentVolume**: Provides persistent storage
- **Service**: Enables internal communication

Think of this layer as a secure filing cabinet that:
- Keeps data safe and persistent
- Is accessible only within the cluster
- Maintains configuration separately from code

### 2. Backend Layer
The Node.js API is managed by:
- **Deployment**: Runs and manages API pods
- **Service**: Routes internal traffic
- Uses ConfigMap and Secret for database connection

This layer acts like a data processor that:
- Handles API requests
- Communicates with the database
- Can scale based on demand

### 3. Frontend Layer
The React frontend is managed through:
- **Deployment**: Runs the web interface
- **Service**: Manages pod access
- **Ingress**: Handles external access

Think of this as the user interface that:
- Presents the Todo application
- Communicates with the backend
- Is accessible from outside the cluster

## How It All Works Together

### 1. User Interaction Flow
When a user interacts with the application:
1. Request hits the Ingress
2. Ingress routes to appropriate service
3. Service directs to correct pod
4. Pod processes the request

### 2. Data Flow
For creating a todo:
1. User enters todo item
2. Frontend sends request through Ingress
3. Backend pod receives request
4. Backend connects to database
5. Data is stored persistently

## Common Kubernetes Patterns Used

### 1. Configuration Management
- ConfigMaps for non-sensitive settings
- Secrets for sensitive data
- Environment variables injection

### 2. Service Discovery
- Internal DNS resolution
- Service abstraction
- Pod-to-pod communication

### 3. State Management
- Persistent storage for database
- Stateless application pods
- Data persistence across restarts

## Getting Started

1. Prerequisites:
   - Minikube installed
   - kubectl configured
   - Docker installed

2. Clone the repository:
   ```bash
   git clone https://github.com/akamasine/Demo-Todo.git
   ```

3. Follow the setup instructions in the repository's README.md

## Key Takeaways

1. **Separation of Concerns**
   - Each component has specific responsibilities
   - Configuration separate from code
   - Clear boundaries between services

2. **Kubernetes Benefits**
   - Declarative configuration
   - Automatic scaling
   - Self-healing capabilities
   - Easy service discovery

3. **Best Practices**
   - Use appropriate resource types
   - Implement proper health checks
   - Maintain security with Secrets
   - Keep configurations separate

## Conclusion
This Todo application demonstrates fundamental Kubernetes concepts in a practical way. While simple, it showcases patterns used in larger applications.

## Next Steps
- Add authentication
- Implement monitoring
- Set up CI/CD pipeline
- Add database replication

## Resources
- [Project Repository](https://github.com/akamasine/Demo-Todo)
- [Kubernetes Documentation](https://kubernetes.io/docs/)
- [Minikube Documentation](https://minikube.sigs.k8s.io/docs/)

