# Academic Assignment & Grading Management System

A full-stack RESTful web application for managing the complete academic assignment lifecycle. The system enables teachers to create and manage assignments, review student submissions, provide grades and feedback, and monitor class performance. Students can view available assignments, submit coursework, work with partners, and track their grades and feedback.

The application is built using a **Layered Architecture (Model-Service-Router)** and implements **JWT authentication, Role-Based Access Control (RBAC), input validation, custom middleware, and centralized error handling**.

---

## Table of Contents

* [Tech Stack & Architecture](#-tech-stack--architecture)
* [Key Features](#-key-features)
* [Database Schemas & Virtuals](#-database-schemas--virtuals)
* [Middleware Pipeline](#-middleware-pipeline)
* [API Reference](#-api-reference)
* [Execution & Setup Guide](#-execution--setup-guide)
* [Client Usage & Demo Credentials](#-client-usage--demo-credentials)

---

## Tech Stack & Architecture

### Core Technologies

* **Backend:** Node.js, TypeScript, Express.js
* **Database:** MongoDB, Mongoose
* **Authentication & Security:** JSON Web Tokens (JWT), bcrypt
* **Frontend:** React, JavaScript/TypeScript, HTML5, CSS3

### Layered Architecture

The application follows a layered architecture that separates responsibilities across the following components:

* **Router Layer (`/routes`):** Handles HTTP routing, request parameters, and middleware binding.
* **Service Layer (`/services`):** Contains business logic, database operations, and statistical calculations.
* **Model Layer (`/models`):** Defines Mongoose schemas, relationships, validation rules, and virtual fields.
* **Middleware Layer (`/middleware`):** Handles logging, authentication, authorization, validation, and error handling.

This separation improves **maintainability, scalability, and code organization**.

---

## Key Features

### Security & Role-Based Access Control

* JWT-based authentication with Bearer tokens.
* Role-Based Access Control (RBAC) for **Teacher** and **Student** users.
* Protected routes based on user roles.
* Password hashing using **bcrypt**.
* Registration validation and duplicate account prevention.
* Centralized error handling with appropriate HTTP status codes.

### Teacher Capabilities

* Create and publish assignments.
* Define assignment descriptions and deadlines.
* View student submissions.
* Access student and assignment information through populated MongoDB relationships.
* Assign grades and provide detailed feedback.
* Calculate and review overall class averages.

### Student Capabilities

* View available assignments.
* Submit assignments using GitHub repository links.
* Submit assignments individually or with an optional partner.
* View personal submissions.
* Track grades and teacher feedback.
* View overall class performance statistics.

---

## Database Schemas & Virtuals

| Model          | Primary Fields                                                            | Relationships        | Virtual Fields |
| :------------- | :------------------------------------------------------------------------ | :------------------- | :------------- |
| **User**       | `userId`, `name`, `email`, `password`, `role`                             | —                    | —              |
| **Assignment** | `title`, `description`, `dueDate`, `createdDate`                          | —                    | `isOpen`       |
| **Submission** | `assignmentId`, `studentId`, `githubLink`, `partner`, `grade`, `feedback` | `Assignment`, `User` | `isGraded`     |

### Virtual Fields

* **`Assignment.isOpen`** — Determines whether an assignment is still open for submission based on its deadline.
* **`Submission.isGraded`** — Determines whether a submission has received a grade.

---

## Middleware Pipeline

The application uses a custom middleware pipeline to handle common backend concerns:

1. **Global Logger Middleware**
   Logs the HTTP method, URL path, and timestamp for incoming requests.

2. **Registration Validation Middleware**
   Validates registration data and prevents duplicate accounts.

3. **Authentication Middleware (`authMiddleware`)**
   Extracts and verifies the Bearer JWT token and attaches the authenticated user's information to the request.

4. **Authorization Middleware (`roleMiddleware`)**
   Verifies that the authenticated user has the required role for the requested endpoint.

5. **Centralized Error Handling Middleware**
   Handles application errors and returns structured responses with appropriate HTTP status codes such as `400`, `401`, `403`, `404`, and `500`.

---

## API Reference

The backend exposes RESTful API endpoints for:

* User registration and authentication
* Assignment management
* Student submissions
* Grading and feedback
* Student and class statistics

All protected endpoints require a valid JWT authentication token and, where applicable, the appropriate user role.

---

## Execution & Setup Guide

### 1. Backend Server Setup

Open a terminal in the **backend project root** and run:

```bash
# Install dependencies
npm install

# Start the backend server
npx ts-node src/app.ts
```

The backend server will run on:

**http://localhost:5000**

---

### 2. Client Setup

Open a **new terminal** in the client project directory and run:

```bash
# Install dependencies
npm install

# Start the client application
npm start
```

The client application will run on:

**http://localhost:3000**

---

### 3. Running the Application

Both the backend and client must be running simultaneously.

1. Start the backend server in one terminal.
2. Start the client application in a second terminal.
3. Open the application in your browser:

**http://localhost:3000**

---

## Client Usage & Demo Credentials

### Registration

New users can create an account by selecting:

**"Register"**

and completing the registration form.

### Demo Users

The project includes pre-seeded users for testing the different roles.

#### Teacher

**Email:** `alice@gmail.com`
**Password:** `123 alice`

The Teacher account provides access to:

* Create assignments
* View student submissions
* Grade submissions
* Provide feedback
* View class statistics

#### Student

**Email:** `shir@gmail.com`
**Password:** `1234shir`

The Student account provides access to:

* View available assignments
* Submit assignments
* View personal submissions
* View grades and feedback
* View class averages

---

## Project Highlights

This project demonstrates practical experience with:

* Full-stack web development
* RESTful API design
* TypeScript and Node.js
* MongoDB and Mongoose
* Layered software architecture
* JWT authentication
* Role-Based Access Control (RBAC)
* Middleware design
* Data validation
* Error handling
* Database relationships and aggregation
* Teacher/student workflow management
