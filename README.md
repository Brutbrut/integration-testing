# 🧪 Integration Testing Guide

Integration Testing is a type of software testing where individual units or components are combined and tested as a group. This guide outlines everything you need to know, including approaches, tools, best practices, and examples.

---

## 📌 What is Integration Testing?

Integration Testing focuses on verifying that modules or services within a system interact correctly. While unit testing checks isolated modules, integration testing ensures **data flow and communication** between those modules is accurate and reliable.

---

## 🎯 Objectives

- Verify module interactions and data exchange.
- Detect interface and communication issues early.
- Ensure combined units perform as expected.
- Reduce bugs before system-level testing.

---

## 🏗️ Integration Testing Approaches

### 🔹 Big Bang Integration
- All modules are combined and tested together.
- ✅ Simple setup.
- ❌ Debugging issues is difficult.

### 🔹 Incremental Integration
Gradually integrate and test modules one at a time.

#### Top-Down
- Starts from top-level modules.
- Uses **stubs** for unready lower modules.

#### Bottom-Up
- Starts from bottom-level modules.
- Uses **drivers** for unready upper modules.

#### Sandwich (Hybrid)
- Combines Top-Down and Bottom-Up.

---

## 🔧 Types of Integration Testing

| Type                        | Description                                                                 |
|-----------------------------|-----------------------------------------------------------------------------|
| **Functional Integration**  | Checks if combined modules behave correctly.                               |
| **Thread Testing**          | Tests specific tasks from end to end.                                      |
| **Backbone Integration**    | Focuses on key system control flow.                                        |
| **Call Integration**        | Validates function/method calls between modules.                           |

---

## 🛠️ Tools Commonly Used

| Tool         | Purpose                                  |
|--------------|------------------------------------------|
| JUnit/TestNG | Java unit/integration testing            |
| NUnit        | .NET unit/integration testing            |
| Postman      | API integration testing                  |
| SoapUI       | Web service testing                      |
| Selenium     | UI/web module integration testing        |
| WireMock     | Service virtualization/mocking           |
| Jenkins      | CI automation for running integration tests |

---

## ✅ Best Practices

- Identify and document all integration points.
- Use **mocks, stubs, and drivers** to simulate unready modules.
- Automate integration tests in your CI/CD pipeline.
- Test **critical workflows** and **data paths**.
- Isolate and keep tests repeatable and independent.
- Maintain clean logs and detailed reports.

---

## 🔄 Testing Levels Overview

| Level              | Description                                |
|--------------------|--------------------------------------------|
| **Unit Testing**   | Tests individual modules or functions.     |
| **Integration Testing** | Tests interaction between modules.       |
| **System Testing** | Tests the whole system functionality.      |
| **Acceptance Testing** | Validates against business/user requirements. |

---

## 🧠 Common Issues Caught

- Incorrect/missing data passed between modules.
- Data format or type mismatches.
- Broken or misconfigured APIs.
- Latency or timeout errors.
- Unauthorized access or permission issues.

---

## 💡 Example Scenario: E-Commerce Website

**Modules:**
- `User Login`
- `Shopping Cart`
- `Payment Gateway`

**Integration Test Cases:**
- Verify user login session is maintained in cart.
- Validate cart total is passed correctly to payment.
- Ensure order confirmation triggers email notification.

---
# Postman API Sample (Register/Login/Profile)
## 📝 Step 1: Register a New User
Request Type: POST

Endpoint: https://api.example.com/register

Headers:

Content-Type: application/json

### Body (JSON):
```javascript
{
  "name": "Bruce QA",
  "email": "bruce.qa@example.com",
  "password": "SecureP@ss123"
}

### Tests Tab:

```javascript
pm.test("Status code is 201", function () {
    pm.response.to.have.status(201);
});

pm.test("Response contains user ID", function () {
    const jsonData = pm.response.json();
    pm.expect(jsonData).to.have.property("userId");
    pm.environment.set("userId", jsonData.userId);
});

---

## 📝 Step 2: Login User
Request Type: POST

Endpoint: https://api.example.com/login

Headers:

Content-Type: application/json

### Body (JSON):

```javascript
{
  "email": "bruce.qa@example.com",
  "password": "SecureP@ss123"
};

---

### Tests Tab:
```javascript
pm.test("Login status is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Token is returned", function () {
    const jsonData = pm.response.json();
    pm.expect(jsonData).to.have.property("token");
    pm.environment.set("authToken", jsonData.token);
});

---

## 📝 Step 3: Get User Profile
Request Type: GET

Endpoint: https://api.example.com/profile

Headers:

Authorization: Bearer {{authToken}}

### Tests Tab:

```javascript
pm.test("Profile status is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Profile contains correct user data", function () {
    const jsonData = pm.response.json();
    pm.expect(jsonData.email).to.eql("bruce.qa@example.com");
    pm.expect(jsonData.name).to.eql("Bruce QA");
});

---

## 📦 Environment Variables Used
Variable	Description
authToken	JWT token saved from login
userId	User ID from registration step

📁 How to Organize in Postman
Create a collection named User Flow - Integration Test and add three requests:

Register User

Login User

Get Profile

Use a Postman environment with authToken and userId as variables for seamless integration between requests.

---

# 🧪 Postman API Test Command Reference

| **Category**            | **Purpose**                             | **Command** |
|-------------------------|-----------------------------------------|-------------|
| ✅ **Status Check**     | Validate HTTP 200 OK                    | `pm.response.to.have.status(200);` |
|                         | Validate 401 Unauthorized               | `pm.response.to.have.status(401);` |
|                         | Validate response time < 500ms          | `pm.expect(pm.response.responseTime).to.be.below(500);` |
| 📦 **Body Validation**  | Parse JSON response                     | `let jsonData = pm.response.json();` |
|                         | Check value of a field                  | `pm.expect(jsonData.email).to.eql("test@example.com");` |
|                         | Check field exists                      | `pm.expect(jsonData).to.have.property("userId");` |
| 🔁 **Looping**          | Check each item in array has "email"    | `jsonData.forEach(u => pm.expect(u).to.have.property("email"));` |
| 📌 **Environment Vars** | Set environment variable                | `pm.environment.set("token", jsonData.token);` |
|                         | Unset environment variable              | `pm.environment.unset("token");` |
| 🌍 **Global Vars**      | Set global variable                     | `pm.globals.set("sessionId", "12345");` |
|                         | Unset global variable                   | `pm.globals.unset("sessionId");` |
| 🔍 **Headers**          | Validate `Content-Type` is JSON         | `pm.response.to.have.header("Content-Type", "application/json");` |
|                         | Get a header value                      | `pm.response.headers.get("Authorization");` |
| 🧹 **Utilities**        | Print to console                        | `console.log("Token:", pm.environment.get("token"));` |
|                         | Delay execution (pre-request only)      | `setTimeout(() => {}, 2000);` |

---

### 💡 Example Use in Tests Tab

```javascript
pm.test("Status is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Email is correct", function () {
    const jsonData = pm.response.json();
    pm.expect(jsonData.email).to.eql("test@example.com");
});
