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

---

## Database Design

### Key Entities & Relationships

### 1. Users
**Fields:**
- `id` (Primary Key)
- `email` (Unique)
- `password_hash` (Encrypted)
- `name`
- `role` (Host/Guest)

**Relationships:**
- One-to-Many with `Properties` (A user can list multiple properties)
- One-to-Many with `Bookings` (A user can make multiple bookings)
- One-to-Many with `Reviews` (A user can write multiple reviews)

### 2. Properties
**Fields:**
- `id` (Primary Key)
- `title`
- `description`
- `price_per_night`
- `location` (Address/Coordinates)
- `host_id` (Foreign Key → Users)

**Relationships:**
- Many-to-One with `Users` (A property belongs to one host)
- One-to-Many with `Bookings` (A property can have multiple bookings)
- One-to-Many with `Reviews` (A property can receive multiple reviews)

### 3. Bookings
**Fields:**
- `id` (Primary Key)
- `check_in_date`
- `check_out_date`
- `total_price`
- `status` (Confirmed/Cancelled/Completed/Pending)
- `guest_id` (Foreign Key → Users)
- `property_id` (Foreign Key → Properties)

**Relationships:**
- Many-to-One with `Users` (A booking belongs to one guest)
- Many-to-One with `Properties` (A booking is for one property)
- One-to-One with `Payments` (Each booking has one payment)

### 4. Reviews
**Fields:**
- `id` (Primary Key)
- `rating` (1-5)
- `comment`
- `created_at`
- `guest_id` (Foreign Key → Users)
- `property_id` (Foreign Key → Properties)

**Relationships:**
- Many-to-One with `Users` (A review is written by one user)
- Many-to-One with `Properties` (A review is for one property)

### 5. Payments
**Fields:**
- `id` (Primary Key)
- `amount`
- `payment_method` (FlutterWave/Airtel Money/ MTN Money)
- `transaction_id`
- `status` (Success/Failed/Pending)
- `booking_id` (Foreign Key → Bookings)

**Relationships:**
- One-to-One with `Bookings` (A payment processes one booking)

## Relationship Summary

| Relationship | Description |
|--------------|-------------|
| Users ↔ Properties | Hosts can own multiple properties |
| Properties ↔ Bookings | Properties can be booked multiple times |
| Users ↔ Bookings | Guests can make multiple bookings |
| Properties ↔ Reviews | Properties can receive multiple reviews |
| Bookings ↔ Payments | Each booking has one associated payment |

---

## Feature Breakdown

### 1. API Documentation
**OpenAPI Standard**  
All backend APIs follow the OpenAPI specification, providing clear, standardized documentation that makes integration straightforward for frontend developers and third-party services. This includes detailed descriptions of endpoints, request/response formats, and authentication requirements.

### 2. User Authentication
**Endpoints**: `/users/`, `/users/{user_id}/`  
**Features**:
- **User Registration**  
  - Enables new users to sign up with email/password  
  - Includes email verification to prevent fake accounts  
  - Creates base profile for accessing booking/hosting features  

- **Login/Logout**  
  - Secure JWT token-based authentication  
  - Protected API endpoints for sensitive actions  
  - Session management for persistent logins  

- **Profile Management**  
  - Edit personal information, profile photos, and preferences  
  - Hosts: Submit ID verification documents  
  - Guests: Save payment methods and travel preferences  

- **Security Features**  
  - Password hashing
  - Rate limiting on auth endpoints  
  - Automatic token expiration 

### 3. Property Management
**Endpoints**: `/properties/`, `/properties/{property_id}/`  
**Features**:
- **Create Listings**  
  - Allows hosts to add new properties with details (title, description, price, amenities)  
  - Supports multiple media uploads (photos/videos) for showcasing spaces  
  - Sets availability calendar for booking management  

- **Update Listings**  
  - Enables hosts to modify property details and pricing  
  - Real-time sync ensures booking systems show accurate information  
  - Version history tracks changes for dispute resolution  

- **Retrieve Listings**  
  - Provides detailed property views for guests with filters (price, location, amenities)  
  - Optimized search with geolocation for nearby properties  
  - Caches frequent queries to improve performance  

- **Delete Listings**  
  - Soft-delete functionality preserves booking history  
  - Automated notifications to guests with active reservations  
  - Option to temporarily deactivate instead of permanent removal 

### 4. Booking System
**Endpoints**: `/bookings/`, `/bookings/{booking_id}/`  
**Features**:
- **Make Bookings**  
  - Enables guests to reserve properties for specific dates with real-time availability checks  
  - Calculates total costs including fees and taxes automatically  
  - Integrates with payment system to confirm reservations instantly  

- **Update Bookings**  
  - Allows modifications to dates or guest counts (when permitted by host)  
  - Automatically adjusts pricing and sends notifications to both parties  
  - Maintains change history for transparency and dispute resolution  

- **Manage Bookings**  
  - Provides unified dashboard for hosts/guests to view all reservations  
  - Handles check-in/check-out procedures with digital confirmations  
  - Supports cancellation policies with automated refund processing

### 5. Payment Processing
**Endpoints**: `/payments/`  
**Features**:
- **Secure Transactions**  
  - Integrates with Stripe/PayPal to process credit card and digital wallet payments  
  - Encrypts all sensitive payment data using PCI-compliant standards  
  - Provides instant payment confirmation for completed bookings  

- **Booking Payments**  
  - Automatically charges guests upon reservation confirmation  
  - Calculates and itemizes totals (rental fees, taxes, cleaning charges)  
  - Supports partial payments/deposits based on host policies  

- **Refund Processing**  
  - Automates refunds according to cancellation policies  
  - Tracks payment status (completed/refunded/failed) for all transactions  
  - Notifies both parties via email when payments are processed  

- **Payouts to Hosts**  
  - Transfers earnings to host accounts after stay completion  
  - Deducts platform fees automatically  
  - Provides earnings reports with transaction history  

### 6. Review System
**Endpoints**: `/reviews/`, `/reviews/{review_id}/`  
**Features**:
- **Post Reviews**  
  - Enables guests to submit ratings (1-5 stars) and written feedback after their stay  
  - Ensures authenticity by restricting reviews to verified guests who completed bookings  
  - Allows photo attachments to share genuine experiences  

- **Manage Reviews**  
  - Provides hosts with ability to respond to guest reviews publicly  
  - Includes reporting system for inappropriate/fake content moderation  
  - Implements edit/delete options within limited time window  

- **Review Display**  
  - Calculates and showcases aggregate ratings on property listings  
  - Offers sorting/filtering options (newest, highest rated)  
  - Highlights featured reviews with verified badges

### 7. Database Optimizations 
**Features**:
- **Indexing**  
  - Implements optimized indexes on primary keys, foreign keys, and search fields  
  - Dramatically speeds up queries for property searches and user data retrieval  
  - Reduces database load during peak traffic periods  

- **Caching**  
  - Uses Redis to cache frequently accessed data like property listings and search results  
  - Implements smart cache invalidation when underlying data changes  
  - Improves response times by reducing repetitive database queries  

- **Query Optimization**  
  - Analyzes and refines slow-performing queries using EXPLAIN  
  - Implements efficient pagination for large datasets  
  - Balances normalization with strategic denormalization where beneficial  

- **Monitoring**  
  - Tracks database performance metrics in real-time  
  - Alerts for slow queries and cache misses  
  - Provides insights for continuous performance tuning