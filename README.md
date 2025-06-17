# airbnb-clone-project
An AirBnb Clone repo

## Overview

A fully functioning Airbnb backend clone built with Django, featuring user authentication, property listings, bookings, payments, and reviews. Designed to mirror core Airbnb functionalities with a focus on security, performance, and scalability.

---

## Project Goals

1. **User Management**: Secure registration, authentication, and profile management.
2. **Property Management**: CRUD operations for property listings with search/filter capabilities.
3. **Booking System**: Reserve properties, manage dates, and handle cancellations.
4. **Payment Processing**: Payment system for transactions with payment history tracking.
5. **Review System**: Submit reviews and ratings for properties.
6. **Database Optimization**: Efficient data retrieval and storage.

---

## Tech Stack

- **Django**
- **Django REST Framework** 
- **PostgreSQL**
- **GraphQL**
- **Celery** 
- **Redis**
- **Docker**
- **CI/CD Pipelines**

---

## Team Roles

**1. Backend Developer**: 
   - The backend developer designs and develops server-side application logic and architecture. They implement API endpoints to support client-side functionality while creating and maintaining database schemas that align with the specified business requirements. Their work includes developing authentication and authorization systems, integrating third-party services and payment processing systems, as well as optimizing application performance and scalability.

**2. Database Administrator**:
   - The database administrator designs and maintains the database structure for optimal performance by implementing data storage solutions and access patterns. They monitor and tune database performance through indexing and query optimization while ensuring data security, integrity, and proper backup procedures. Their responsibilities include planning for database scalability as the user base grows and collaborating with developers on schema design and migrations.

**3. DevOps Engineer**:
   - The DevOps engineer configures and maintains deployment pipelines while implementing monitoring and alerting systems. They manage cloud infrastructure and container orchestration, automating testing and deployment processes. Their role focuses on ensuring system reliability and uptime while implementing security best practices for infrastructure.

**4. QA Engineer**:
   - The QA engineer develops and executes comprehensive test plans for all system components. They implement automated testing frameworks and identify, document, and track software defects. Their work involves verifying system performance under various conditions while ensuring compliance with quality standards and collaborating with developers to reproduce and resolve issues.   

   ---

## Technology Stack

   - **Django**: Serves as the foundation, handling backend    logic, URL routing, and server-side templating. It organizes the entire web application.
   - **Django REST Framework**: Builds the API layer that allows frontend clients to interact with the backend system. It creates RESTful endpoints for user accounts, property listings, bookings, and other core features. 
   - **PostgreSQL**: Acts as the primary relational database, storing structured data like user profiles, property details, bookings, and payment records with ACID compliance.
   - **GraphQL**: Offers an alternative to REST APIs, enabling clients to request exactly the data they need in single queries. Used for complex data fetching scenarios like combined property+reviews data.
   - **Celery**: Handles background tasks like sending confirmation emails, processing payments asynchronously, and generating reports without blocking or overwhelming the main application.
   - **Redis**: Provides caching for frequently accessed data (like popular listings).
   - **Docker**: Containerizes the application for consistent environments across development, testing, and production. Packages Django, PostgreSQL, Redis, and all other stacks together.
   - **CI/CD Pipelines**: Automates testing (unit/integration), security checks, and deployment processes. Ensures code quality and enables frequent, reliable updates to production.
