---
title: "Web Requests - HTB Academy"
date: 2026-09-27 10:00:00 +0300
categories: [Lab-Challenges]
tags: [HTTP, Web-Requests, HTB-Academy, cURL, APIs, GET, POST, REST, Cybersecurity]
description: "Completed the HTB Academy Web Requests module, exploring HTTP requests, headers, GET, POST, cookies, authentication, and API interaction using cURL and browser developer tools."
---

# Web Requests - HTB Academy

## Challenge Overview

This lab was completed through the Hack The Box Academy **Web Requests** module.

The module focused on understanding how web applications communicate using HTTP requests and responses, teaching both browser-based and command-line techniques for interacting with web servers.

Using **cURL** and browser **Developer Tools**, the exercises explored:

- HTTP Requests
- HTTP Responses
- HTTP Headers
- Authentication
- GET Requests
- POST Requests
- Session Cookies
- REST APIs
- CRUD Operations

---

# Problem Statement

Modern web applications rely heavily on HTTP communication between clients and servers.

The challenge was to understand:

- How browsers communicate with web servers.
- How requests can be observed and manipulated.
- How headers and cookies affect application behavior.
- How APIs exchange information.
- How tools such as cURL can be used to manually interact with web applications.

---

# Objectives

- Learn to interact with web servers using cURL.
- Analyze HTTP requests and responses.
- Inspect HTTP headers.
- Understand GET and POST requests.
- Work with authentication and session cookies.
- Interact with REST-style APIs.
- Practice CRUD operations using HTTP methods.

---

# Tools Used

## Command Line

- cURL

## Browser Tools

- Developer Tools (DevTools)
- Network Tab

## Platform

- Hack The Box Academy

---

# Module 1: cURL Essentials

## Overview

This exercise introduced cURL as a command-line HTTP client capable of interacting directly with web servers.

The objective was to identify the correct endpoint and retrieve information from the application.

### Command Used

```bash
curl <target>/download.php
```

### Result

The target endpoint returned the challenge flag directly.

### Key Learning

Web applications often expose functionality through endpoints that may not be immediately visible through a browser interface.

---

# Module 2: HTTP Requests and Responses

## Overview

This exercise focused on understanding HTTP response headers.

Instead of examining page content, attention was placed on metadata returned by the server.

### Command Used

```bash
curl -I <target>
```

### Information Discovered

Server software:

```text
Apache
```

Version:

```text
2.4.41
```

### Key Learning

Response headers frequently reveal:

- Server Types
- Framework Information
- Application Technologies
- Configuration Details

Such information is often useful during reconnaissance activities.

---

# Module 3: HTTP Headers

## Overview

The objective was to identify hidden activity occurring after page load.

Using the browser Network tab, additional requests initiated by the application became visible.

### Activity Performed

1. Open Browser Developer Tools.
2. Navigate to the Network tab.
3. Reload the page.
4. Inspect requests and responses.

### Result

A background request retrieved the required information.

### Key Learning

Important functionality may occur asynchronously after page load.

Monitoring network traffic often reveals:

- Hidden API calls
- Background requests
- Authentication workflows
- Additional endpoints

---

# Module 4: GET Requests

## Overview

A search application appeared broken through the user interface.

Investigation using Developer Tools revealed the actual request structure and authentication requirements.

### Observations

The application used:

```http
GET
```

along with:

```http
Authorization: Basic
```

authentication headers.

### Command Used

```bash
curl "<target>/search.php?search=flag" \
-H "Authorization: Basic <credentials>"
```

### Outcome

The search returned the expected result once the correct authorization header was included.

### Key Learning

User interfaces may fail while backend functionality remains operational.

Inspecting network requests helps identify:

- Authentication mechanisms
- Request parameters
- Hidden functionality

---

# Module 5: POST Requests

## Overview

This exercise introduced authenticated sessions and cookie handling.

The challenge required:

1. Logging into the application.
2. Saving the session cookie.
3. Reusing the cookie in future requests.

---

## Step 1 — Login Request

### Command

```bash
curl -X POST \
-d "username=admin&password=admin" \
-c cookies.txt \
<target>
```

### Result

A valid session cookie was stored locally.

---

## Step 2 — Authenticated Search

### Command

```bash
curl -X POST \
-d '{"search":"flag"}' \
-H 'Content-Type: application/json' \
-b cookies.txt \
<target>/search.php
```

### Result

The authenticated request returned the expected output.

### Key Learning

Many applications depend on:

- Session Cookies
- Authentication Tokens
- Stateful Communication

A valid session often determines whether functionality can be accessed successfully.

---

# Module 6: CRUD API

## Overview

The final challenge introduced REST-style API interactions using multiple HTTP methods.

CRUD stands for:

| Operation | HTTP Method |
|------------|-------------|
| Create | POST |
| Read | GET |
| Update | PUT |
| Delete | DELETE |

---

## Updating Data

### Command

```bash
curl -X PUT \
<target>/api.php/city/london \
-d '{"city_name":"flag","country_name":"HTB"}'
```

This renamed an existing city record.

---

## Deleting Data

### Command

```bash
curl -X DELETE \
<target>/api.php/city/leeds
```

This removed another city entry.

---

## Reading Data

### Command

```bash
curl <target>/api.php/city/flag
```

The modified resource was retrieved successfully.

### Key Learning

REST APIs expose functionality through predictable HTTP methods and endpoint structures.

Understanding CRUD operations is critical when analyzing modern web applications.

---

# Practical Skills Gained

The module provided practical experience in:

- HTTP Analysis
- Request Inspection
- Header Analysis
- Browser Developer Tools
- Session Management
- Cookie Handling
- API Interactions
- Authentication Mechanisms
- REST Architecture
- Command-Line Web Interaction

---

# Security Relevance

These concepts are fundamental in:

- Web Application Security
- Penetration Testing
- Bug Hunting
- API Security Testing
- Threat Hunting
- Security Assessments

Many vulnerabilities become visible only after understanding:

- Request Structure
- Session Handling
- Authentication Models
- API Functionality

---

# Evidence of Completion

The completion of this module can be verified using the official HTB Academy achievement page.

🔗 **Achievement Badge:**

[HTB Academy Web Requests Achievement](https://academy.hackthebox.com/achievement/2870830/35)

### Completion Summary

- Platform: HTB Academy
- Module: Web Requests
- Status: Completed
- Exercises Completed: 6 Hands-On Labs
- Topics Covered:
  - HTTP Requests & Responses
  - HTTP Headers
  - cURL
  - GET Requests
  - POST Requests
  - Session Cookies
  - CRUD APIs

---

# Key Findings

## Browsers Hide Complexity

Modern web applications often appear simple through the user interface while performing numerous unseen HTTP requests behind the scenes.

---

## Developer Tools Are Essential

Network traffic inspection can reveal:

- Hidden Endpoints
- Authorization Headers
- Session Tokens
- API Calls

---

## Authentication Often Depends on Headers and Cookies

A properly authenticated request frequently requires:

- Authorization Headers
- Session Cookies
- JSON Data
- Specific HTTP Methods

---

## APIs Rely on Standard HTTP Verbs

Most modern web applications expose functionality through:

```text
GET
POST
PUT
DELETE
```

requests.

---

# Skills Demonstrated

- HTTP Fundamentals
- Request Manipulation
- Response Analysis
- Browser DevTools Usage
- Session Management
- Cookie Analysis
- API Enumeration
- REST API Interaction
- cURL Proficiency
- Security Reconnaissance

---

# Key Lessons Learned

- HTTP requests contain valuable information beyond visible page content.
- Response headers frequently disclose server technologies.
- Network traffic often reveals hidden application functionality.
- Cookies are central to session management.
- APIs can be manipulated directly without relying on a browser interface.
- Understanding HTTP improves both troubleshooting and security assessment capabilities.
- cURL is an essential tool for interacting with and testing web applications.

---

# Conclusion

The HTB Academy Web Requests module provided practical experience interacting directly with web applications through HTTP. By using cURL and browser Developer Tools, I learned how to inspect requests, analyze responses, authenticate sessions, manage cookies, and communicate with APIs using standard HTTP methods.

These skills form an essential foundation for web application security testing, API security assessments, penetration testing, and advanced cybersecurity investigations.
