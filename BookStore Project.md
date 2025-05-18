📄 BookStore Project – Interview Question Sheet with Answers
============================================================

This document contains a comprehensive set of interview questions with detailed answers based on your 1.5+ months of work on the BookStore microservices project.

✅ .NET Core & Clean Architecture
--------------------------------

### Basics

1.  **What are the key components of Clean Architecture in .NET Core?**
    
    *   Clean Architecture separates concerns into layers: **Domain**, **Application**, **Infrastructure**, and **Presentation (API)**. Each layer has a specific role and dependencies only flow inward.
        
2.  **How do you enforce separation of concerns in your services?**
    
    *   By using interfaces and dependency injection. Controllers only call Application Services which use Repositories. Business logic resides in the Domain layer.
        
3.  **How do you manage dependency injection in your BookStore microservices?**
    
    *   Using StartupExtensions.cs to register services, repositories, configuration settings, and middleware.
        
4.  **What are IRepository and DbContext and how are they used?**
    
    *   IRepository abstracts data access logic. DbContext from EF Core manages database connections. The repository uses DbContext to perform operations.
        
5.  **What is the difference between synchronous and asynchronous controller actions?**
    
    *   Async actions use async/await to avoid blocking threads. This improves scalability for I/O-bound operations like DB or HTTP calls.
        

### Advanced

1.  **How do you manage domain-driven design (DDD) within your microservices?**
    
    *   We use Entities, Value Objects, Aggregates, and Domain Events. The Domain layer contains business rules and validations.
        
2.  **What is a Value Object and how did you use it in the BookStore domain?**
    
    *   A Value Object is immutable and compares by values, not identity. For example, Price or Email can be implemented as value objects.
        
3.  **How do you implement retries and error handling in service communication?**
    
    *   By using Polly-based retry policies or Kafka retry topics, and logging failed messages to dead-letter topics or queues.
        
4.  **Explain the purpose of using extension methods like StartupExtensions.cs.**
    
    *   It helps modularize service registration logic and keeps the Program.cs clean and maintainable.
        

🔐 Authentication & Authorization
---------------------------------

1.  **What is JWT and how is it generated and validated?**
    
    *   JSON Web Token (JWT) is a compact, URL-safe token. It's generated using a secret and validated by parsing the token and verifying its signature.
        
2.  **How does your custom AuthService handle user login and token issuance?**
    
    *   It verifies credentials, generates JWT using a secret key, and returns it to the client. Claims like userId and role are embedded.
        
3.  **How do you validate JWT in downstream microservices?**
    
    *   Using JwtBearer middleware in ASP.NET Core. Microservices validate the token using the same secret used for signing.
        
4.  **Why did you choose a custom AuthService over Azure AD or OAuth?**
    
    *   For flexibility, full control, and learning. It's suitable for custom roles and claims-based auth.
        
5.  **How do you handle role-based or scope-based authorization in your APIs?**
    
    *   Using \[Authorize(Roles="Admin")\] or custom authorization policies in each microservice.
        

⚙️ Microservices Architecture
-----------------------------

1.  **What are the key benefits of using a Self-Contained System (SCS) model?**
    
    *   Independence in deployment, tech stack, and database. Each SCS can scale and evolve separately.
        
2.  **How do microservices within a single SCA (e.g., Product + Inventory) communicate?**
    
    *   Using Azure Service Bus or Kafka for asynchronous event-based communication.
        
3.  **What messaging pattern do you use for async communication?**
    
    *   Publisher-subscriber pattern. Services publish domain events like ProductCreated which others consume.
        
4.  **Why did you shift from Azure Service Bus to Kafka?**
    
    *   Kafka offers better event streaming and ordering guarantees. It's cloud-native, scalable, and future-proof.
        
5.  **How do you ensure eventual consistency in your microservices?**
    
    *   By designing idempotent consumers and ensuring event delivery guarantees with retries and dead-letter topics.
        

☁️ Azure Services
-----------------

1.  **How did you use Azure App Services for initial deployment?**
    
    *   Used Azure DevOps pipelines to deploy .NET microservices and Angular UI to App Services via zip deployment.
        
2.  **How does Azure API Management (APIM) act as your API gateway?**
    
    *   It centralizes routing, security, throttling, and analytics. APIM exposes all microservices securely.
        
3.  **What is Azure Function Proxy and why did you introduce it for APIM routing?**
    
    *   APIM cannot route directly to DuckDNS. Azure Function Proxy serves as a public HTTPS endpoint pointing to AKS ingress.
        
4.  **How do you provision Azure resources using DevOps pipelines instead of manual steps?**
    
    *   Using Azure CLI scripts in the pipeline to create App Services, Service Bus, Function Apps, etc.
        
5.  **How do you manage environment-specific secrets in Azure?**
    
    *   Secrets are stored in Azure DevOps Library or Key Vault and injected into the pipeline using --from-literal or variables.
        

🛠 DevOps & CI/CD (Azure DevOps)
--------------------------------

1.  **What are the key stages in your Build and Release pipeline?**
    
    *   Build: Restore, Build, Publish, Push Image. Release: IAC → Secret Injection → App Deployment → Ingress → Validation.
        
2.  **How do you inject appsettings or secrets during deployment?**
    
    *   Via kubectl create secret commands using pipeline variables.
        
3.  **How do you automate Azure Function deployment via pipeline?**
    
    *   Use zip deploy to push the Function app along with host.json and proxies.json using Azure CLI.
        
4.  **What steps are required in your IAC pipeline before Kubernetes deployment?**
    
    *   Create namespace, Ingress controller, cert-manager, DNS mapping, TLS issuer.
        
5.  **How did you automate DuckDNS + Ingress + TLS setup in your AKS cluster?**
    
    *   By scripting kubectl commands to install nginx-ingress, cert-manager, and DNS annotations for auto TLS certs.
        

🐳 Docker & 🧠 Kubernetes (AKS)
-------------------------------

1.  **How do you build and tag Docker images for your microservices?**
    
    *   Using docker build -t imageName:tag . and docker push to ACR.
        
2.  **What is the role of Deployment, Service, and Ingress in Kubernetes?**
    
    *   Deployment: Manages replicas and rollout. Service: Exposes pods. Ingress: Handles external routing.
        
3.  **How do you enable HTTPS using cert-manager and ClusterIssuer?**
    
    *   Deploy cert-manager, create ClusterIssuer (ACME), and annotate ingress with cert-manager.io configs.
        
4.  **What steps did you follow to debug a failing Kafka consumer inside AKS?**
    
    *   Port-forward pod, check logs, inspect environment variables, and compare secret names and Kafka config keys.
        
5.  **How do you configure Kubernetes secrets for each microservice securely?**
    
    *   Using pipeline to apply base64 secrets via kubectl apply and inject into deployments as env vars.
        

📦 Kafka (Confluent Cloud)
--------------------------

1.  **What are the advantages of using Kafka over Azure Service Bus?**
    
    *   Kafka is better for stream processing, event ordering, high throughput, and long-term storage.
        
2.  **How do you configure Kafka producers and consumers in your microservices?**
    
    *   Using Confluent.Kafka package and settings injected from secrets (bootstrap servers, credentials).
        
3.  **What retry and dead-letter strategy did you design for failed Kafka messages?**
    
    *   Implemented try-catch with retries. Planned use of dead-letter topics for poison messages.
        
4.  **How do you test and validate Kafka events between ProductService and InventoryService?**
    
    *   Local testing using mock consumers, followed by end-to-end tests in AKS with Kafka logs.
        
5.  **What is the significance of Consumer Group and Offset in Kafka?**
    
    *   Consumer group ensures load balancing. Offset tracks which message was last processed to avoid duplication.
        

💻 Angular UI (Frontend)
------------------------

1.  **How does your Angular UI authenticate with AuthService?**
    
    *   Sends username/password to AuthService → receives JWT → stores in local storage → used in HTTP interceptor.
        
2.  **How do you manage route guards in Angular?**
    
    *   Implement AuthGuard and check for token presence before allowing navigation.
        
3.  **How do you make service calls to backend via APIM?**
    
    *   Angular services call APIM endpoint URLs instead of direct microservices. All CORS handled via APIM.
        
4.  **What Angular components have you built so far in the BookStore UI?**
    
    *   LoginComponent, ProductListComponent, InventoryComponent. Each connects to corresponding backend.
        
5.  **How did you resolve CORS and proxy issues with Angular and APIM?**
    
    *   Used proxy.config.json for local dev. Enabled CORS in Ingress and APIM policies.
        

📚 BookStore Domain-Specific Scenarios
--------------------------------------

1.  **How does the ProductCreated event trigger inventory updates?**
    
    *   ProductService publishes ProductCreated event to Kafka → InventoryService subscribes and creates stock.
        
2.  **How did you ensure all services are health-checked and observable?**
    
    *   Implemented /health endpoints. Kubernetes liveness and readiness probes added.
        
3.  **What logging strategy are you using across microservices?**
    
    *   Serilog with console sink, structured logging. Can be extended with Seq or Azure Monitor.
        
4.  **How do you simulate local Kafka or test cloud Kafka during dev?**
    
    *   Use Dockerized Kafka for local. For cloud, test via dev environment cluster with Confluent.
        
5.  **How would you scale ProductService independently in production?**
    
    *   Kubernetes horizontal pod autoscaler based on CPU/memory or custom metrics.
        

🎯 System Design & Behavioral
-----------------------------

1.  **Design a scalable architecture for a bookstore app with 1M+ users.**
    
    *   Use SCS with APIM, AuthService, Kafka, CosmosDB, AKS for scaling, and Azure CDN + Angular for UI.
        
2.  **How would you ensure secure communication between microservices?**
    
    *   JWT validation, HTTPS ingress, internal-only services, and mTLS (if needed).
        
3.  **If the InventoryService goes down, how would ProductService continue to operate?**
    
    *   Since they’re decoupled via events, ProductService just publishes to Kafka. Retry/dead-letter will ensure reliability.
        
4.  **How would you design for multitenancy in your current system?**
    
    *   Add tenantId claim in JWT and enforce filters in DB queries. Use APIM policies for tenant routing.
        
5.  **What trade-offs did you make when choosing SCS architecture?**
    
    *   Higher initial complexity and deployment overhead in return for better modularity, autonomy, and scaling.
        

