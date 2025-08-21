# Logio: airbnb-clone-project
This project is a clone of **Airbnb**, built to practice and demonstrate backend web development skills.   The main objective is to replicate core features of Airbnb such as user authentication, property management, booking system, data optimization and reviews, while gaining hands-on experience with modern web technologies. 

## 🎯 Goals
- Design and implement a scalable **backend system** using Django and Django REST Framework.  
- Practice **API development** (REST + GraphQL).  
- Integrate **PostgreSQL** for reliable data management.  
- Implement **asynchronous task handling** with Celery and Redis.  
- Learn containerization with **Docker** for smooth deployment.  
- Set up **CI/CD pipelines** for automated testing and deployment.

- ## ⚙️ Technology Stack
- **Django**: High-level Python web framework for building the backend and RESTful API.  
- **Django REST Framework**: Tools for creating and managing RESTful APIs.  
- **PostgreSQL**: Relational database for structured data storage.  
- **GraphQL**: Flexible and efficient data querying.  
- **Celery**: Task queue for asynchronous background jobs (e.g., notifications, payments).  
- **Redis**: In-memory store for caching and session management.  
- **Docker**: Containerization for consistent environments.  
- **CI/CD Pipelines**: Automated testing and deployment workflows.

- ## 👥 Team Roles
- **Backend Developer**: Responsible for implementing API endpoints, database schemas, and business logic.  
- **Database Administrator**: Manages database design, indexing, and optimizations.  
- **DevOps Engineer**: Handles deployment, monitoring, and scaling of the backend services.  
- **QA Engineer**: Ensures the backend functionalities are thoroughly tested and meet quality standards.  

## 📈 API Documentation Overview
- **REST API**: Documented using the **OpenAPI standard**. Includes endpoints for users, properties, bookings, and payments.  
- **GraphQL API**: Provides a flexible query language for retrieving and manipulating data.  

## 📌 Endpoints Overview

### 🔹 Users
- `GET /users/` – List all users  
- `POST /users/` – Create a new user  
- `GET /users/{user_id}/` – Retrieve a specific user  
- `PUT /users/{user_id}/` – Update a specific user  
- `DELETE /users/{user_id}/` – Delete a specific user  

### 🔹 Properties
- `GET /properties/` – List all properties  
- `POST /properties/` – Create a new property  
- `GET /properties/{property_id}/` – Retrieve a specific property  
- `PUT /properties/{property_id}/` – Update a specific property  
- `DELETE /properties/{property_id}/` – Delete a specific property  

### 🔹 Bookings
- `GET /bookings/` – List all bookings  
- `POST /bookings/` – Create a new booking  
- `GET /bookings/{booking_id}/` – Retrieve a specific booking  
- `PUT /bookings/{booking_id}/` – Update a specific booking  
- `DELETE /bookings/{booking_id}/` – Delete a specific booking  

### 🔹 Payments
- `POST /payments/` – Process a payment  

### 🔹 Reviews
- `GET /reviews/` – List all reviews  
- `POST /reviews/` – Create a new review  
- `GET /reviews/{review_id}/` – Retrieve a specific review  
- `PUT /reviews/{review_id}/` – Update a specific review  
- `DELETE /reviews/{review_id}/` – Delete a specific review

- ## 🗄️ Database Design

The database is designed to support the core functionality of the Lodgio platform. Below are the key entities, their important fields, and relationships.

### 🔹 Users
- `id` (Primary Key)  
- `name`  
- `email` (unique)  
- `password_hash`  
- `role` (guest, host, admin)  

**Relationships:**  
- A user can list multiple **properties**.  
- A user can create multiple **bookings**.  
- A user can write multiple **reviews**.  

---

### 🔹 Properties
- `id` (Primary Key)  
- `title`  
- `description`  
- `location`  
- `price_per_night`  
- `owner_id` (Foreign Key → Users.id)  

**Relationships:**  
- A property belongs to one **user** (host).  
- A property can have multiple **bookings**.  
- A property can have multiple **reviews**.  

---

### 🔹 Bookings
- `id` (Primary Key)  
- `user_id` (Foreign Key → Users.id)  
- `property_id` (Foreign Key → Properties.id)  
- `start_date`  
- `end_date`  
- `status` (pending, confirmed, cancelled)  

**Relationships:**  
- A booking is created by a **user** (guest).  
- A booking belongs to a **property**.  
- A booking may have an associated **payment**.  

---

### 🔹 Reviews
- `id` (Primary Key)  
- `user_id` (Foreign Key → Users.id)  
- `property_id` (Foreign Key → Properties.id)  
- `rating` (1–5)  
- `comment`  

**Relationships:**  
- A review is written by a **user**.  
- A review belongs to a **property**.  

---

### 🔹 Payments
- `id` (Primary Key)  
- `booking_id` (Foreign Key → Bookings.id)  
- `amount`  
- `payment_method` (credit card, PayPal, etc.)  
- `status` (pending, completed, failed)  

**Relationships:**  
- A payment belongs to one **booking**.

## 🛎️ Feature Breakdown

### 🔹 User Management
Users can register, log in, and manage their accounts. Authentication and authorization ensure secure access, while different user roles (guest, host, admin) define the actions available to each user.

### 🔹 Property Management
Hosts can list properties with details such as title, description, location, and price per night. They can update or remove listings, making it easy to manage available accommodations.

### 🔹 Booking System
Guests can browse properties and make reservations by selecting dates and specifying the number of guests. The system ensures availability is tracked accurately, preventing double bookings.

### 🔹 Reviews & Ratings
Guests can leave reviews and ratings for properties they have stayed in. This feature helps maintain transparency and builds trust between hosts and guests.

### 🔹 Payments
Guests can securely process payments for their bookings. The system supports multiple payment methods, tracks payment status, and links each payment to a booking.

### 🔹 Notifications & Asynchronous Tasks
Using Celery and Redis, the system handles background tasks such as sending booking confirmations, reminders, or payment notifications. This ensures smooth user experience without delays.

### 🔹 API Access
The platform provides both **REST** and **GraphQL APIs**, enabling flexible integration with frontend clients or third-party services. Developers can query, retrieve, and manipulate data efficiently.


## 🔒 API Security

Securing the backend APIs is a critical aspect of the Lodgio project to protect sensitive data, prevent abuse, and ensure a safe experience for all users.  

### 🔹 Authentication
- **Implementation**: Use JWT (JSON Web Tokens) or OAuth2 for secure login and session management.  
- **Why**: Ensures that only verified users can access the platform, protecting user accounts and preventing unauthorized access.  

### 🔹 Authorization
- **Implementation**: Role-based access control (RBAC) to differentiate between guests, hosts, and admins.  
- **Why**: Prevents unauthorized users from performing restricted actions, such as modifying someone else’s bookings or accessing admin-only features.  

### 🔹 Data Protection
- **Implementation**: Encrypt sensitive data (e.g., passwords with hashing, payments with secure gateways, HTTPS for transport).  
- **Why**: Protects user credentials, payment details, and personal information from being exposed or stolen.  

### 🔹 Rate Limiting & Throttling
- **Implementation**: Apply request limits per user/IP to prevent brute-force attacks and API abuse.  
- **Why**: Ensures service availability and protects the backend from denial-of-service (DoS) attacks.  

### 🔹 Input Validation & Sanitization
- **Implementation**: Validate and sanitize all inputs before processing.  
- **Why**: Prevents injection attacks (SQL injection, XSS) and ensures only valid data enters the system.  

### 🔹 Monitoring & Logging
- **Implementation**: Log all critical actions (e.g., login attempts, failed payments, suspicious activity) with monitoring tools.  
- **Why**: Helps detect security breaches early

- ## ⚙️ CI/CD Pipeline

Continuous Integration (CI) and Continuous Deployment (CD) pipelines automate the process of building, testing, and deploying code changes. This ensures that new features or bug fixes are integrated smoothly without breaking the existing system, improving overall code quality and accelerating development.  

For the Lodgio project, CI/CD pipelines can:  
- Automatically run unit tests and API tests whenever code is pushed to the repository.  
- Build and deploy Docker containers to development or production environments.  
- Detect issues early and provide immediate feedback to developers.  

**Tools used**: GitHub Actions for workflow automation, Docker for containerized deployment, and optionally Redis and Celery integration testing within the pipelines.  

