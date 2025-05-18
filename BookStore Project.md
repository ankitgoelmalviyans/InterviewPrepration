# 📄 BookStore Project – Interview Question Sheet with Answers

This document contains a comprehensive set of interview questions with detailed answers based on your 1.5+ months of work on the BookStore microservices project.

---

## ✅ .NET Core & Clean Architecture

### Basics
1. **What are the key components of Clean Architecture in .NET Core?**
   - Clean Architecture separates concerns into layers: **Domain**, **Application**, **Infrastructure**, and **Presentation (API)**. Each layer has a specific role and dependencies only flow inward.

2. **How do you enforce separation of concerns in your services?**
   - By using interfaces and dependency injection. Controllers only call Application Services which use Repositories. Business logic resides in the Domain layer.

3. **How do you manage dependency injection in your BookStore microservices?**
   - Using `StartupExtensions.cs` to register services, repositories, configuration settings, and middleware.

4. **What are `IRepository` and `DbContext` and how are they used?**
   - `IRepository` abstracts data access logic. `DbContext` from EF Core manages database connections. The repository uses `DbContext` to perform operations.

5. **What is the difference between synchronous and asynchronous controller actions?**
   - Async actions use `async/await` to avoid blocking threads. This improves scalability for I/O-bound operations like DB or HTTP calls.

### Advanced
6. **How do you manage domain-driven design (DDD) within your microservices?**
   - We use Entities, Value Objects, Aggregates, and Domain Events. The Domain layer contains business rules and validations.

7. **What is a Value Object and how did you use it in the BookStore domain?**
   - A Value Object is immutable and compares by values, not identity. For example, `Price` or `Email` can be implemented as value objects.

8. **How do you implement retries and error handling in service communication?**
   - By using Polly-based retry policies or Kafka retry topics, and logging failed messages to dead-letter topics or queues.

9. **Explain the purpose of using extension methods like `StartupExtensions.cs`.**
   - It helps modularize service registration logic and keeps the `Program.cs` clean and maintainable.

---

## 🔐 Authentication & Authorization

1. **What is JWT and how is it generated and validated?**
   - JSON Web Token (JWT) is a compact, URL-safe token. It's generated using a secret and validated by parsing the token and verifying its signature.

... (Truncated for brevity in the code block)
