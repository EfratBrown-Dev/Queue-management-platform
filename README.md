#  Academic Assignment & Grading Management System

A robust, full-stack RESTful web application built with **Node.js**, **TypeScript**, **Express**, and **MongoDB**, featuring a **React / HTML5** client interface. The system automates the assignment lifecycle—enabling teachers to publish coursework, grade submissions, and review class metrics, while allowing students to submit projects, work with partners, and track their academic performance.

Designed using a **Layered Architecture (Model-Service-Router)**, the application enforces strict Role-Based Access Control (RBAC), custom middleware pipelines, data validation, and centralized error handling.

---

##  Table of Contents
- [Tech Stack & Architecture](#-tech-stack--architecture)
- [Key Features](#-key-features)
- [Database Schemas & Virtuals](#-database-schemas--virtuals)
- [Middleware Pipeline](#-middleware-pipeline)
- [API Reference](#-api-reference)
- [Execution & Setup Guide](#-execution--setup-guide)
- [Client Usage & Pre-Seeded Credentials](#-client-usage--pre-seeded-credentials)

---

##  Tech Stack & Architecture

### **Core Technologies**
* **Backend:** Node.js, TypeScript (ES6+), Express.js
* **Database:** MongoDB, Mongoose ODM
* **Security:** JSON Web Tokens (JWT), `bcrypt` password hashing
* **Frontend:** React / HTML5, CSS3, JavaScript (ES6+)

### **Layered Architecture (Model-Service-Router)**
* **Router Layer (`/routes`):** Manages HTTP routing, parameter extraction, and middleware binding.
* **Service Layer (`/services`):** Encapsulates business rules, statistical computations, and MongoDB queries.
* **Model Layer (`/models`):** Defines Mongoose schemas, relationships (`ref`), constraints, and virtual fields.
* **Middleware Layer (`/middleware`):** Handles global logging, payload validation, JWT authentication, RBAC authorization, and error catching.

---

##  Key Features

###  Security & Role-Based Access
* **JWT Authentication:** Secure token generation and header verification.
* **Role-Based Access Control:** Strict route segregation between `Teacher` and `Student` users.
* **Input Validation:** Enforced password complexity (8+ chars, letters and numbers) and duplicate account checks.

###  Teacher Role Capabilities
* **Assignment Creation:** Publish assignments with dynamic deadline checks (`isOpen` virtual field).
* **Submission Review:** View student submissions with populated student and assignment data (`populate`).
* **Grading & Feedback:** Assign grades and detailed feedback by student ID and assignment ID.
* **Analytics:** Compute overall class averages through service aggregation logic.

###  Student Role Capabilities
* **Coursework Navigation:** View all open assignments available for submission.
* **Submission Portal:** Submit GitHub repository links with support for optional team partners (`partnerId`).
* **Grade Tracking:** Review personal grades and feedback (as primary submitter or tagged partner) alongside overall class averages.

---

##  Database Schemas & Virtuals

| Model | Primary Fields | Relationships (`ref`) | Dynamic Virtual Fields |
| :--- | :--- | :--- | :--- |
| **User** | `userId`, `name`, `email`, `password`, `role` | — | Encrypted password storage, unique email/userId validation, role enum (`Student` / `Teacher`). |
| **Assignment** | `title`, `description`, `dueDate`, `createdDate` | — | **`isOpen`**: Computed boolean evaluating if `dueDate >= Date.now()`. |
| **Submission** | `assignmentId`, `studentId`, `githubLink`, `partner`, `grade`, `feedback` | `Assignment`, `User` | **`isGraded`**: Computed boolean evaluating if `grade` field exists. |

---

##  Middleware Pipeline

1. **Global Logger Middleware:** Logs HTTP method, URL path, and timestamp for all incoming requests.
2. **Registration Validation Middleware:** Validates password complexity and checks existing `email` or `userId` records before registering.
3. **Authentication Middleware (`authMiddleware`):** Extracts and verifies Bearer JWT tokens, attaching `_id` and `role` to `req.user`.
4. **Authorization Middleware (`roleMiddleware`):** Verifies `req.user.role` against endpoint requirements (e.g., `'Teacher'`).
5. **Centralized Error Handling Middleware:** Traps runtime errors and returns structured JSON responses with corresponding HTTP status codes (400, 401, 403, 404, 500).

---

## Execution & Setup Guide

### 1. Backend Server Setup

Run the backend server from the **backend project root**:

```bash
# Install dependencies
npm install

# Start the Node.js & TypeScript Server
npx ts-node src/app.ts
```

The backend server will run on:

**http://localhost:5000**

---

### 2. Client Setup

Open a **new terminal** in the client project directory:

```bash
# Install dependencies
npm install

# Start the client
npm start
```

The client application will run on:

**http://localhost:3000**

---

### 3. Running the Application

Both the backend server and the client must be running simultaneously.

1. Start the backend server in one terminal.
2. Start the client in a second terminal.
3. Open the client application in your browser:

**http://localhost:3000**

---

## Client Usage & Pre-Seeded Credentials

### Registration

If a teacher or student wants to create a new account, click:

**"Register"**

and complete the registration form.

### Login

The project includes the following pre-seeded users for testing:

#### Teacher

**Email:** `alice@gmail.com`
**Password:** `123 alice`

After logging in as a teacher, the following features are available:

* Create assignments
* View student submissions
* Grade submissions
* Provide feedback
* View class statistics

#### Student

**Email:** `shir@gmail.com`
**Password:** `1234shir`

After logging in as a student, the following features are available:

* View available assignments
* Submit assignments
* View personal submissions
* View grades and feedback
* View class averages
