# Product Requirements Document (PRD)
# ReqLab — HTTP Request Inspector

**Version:** 1.0  
**Status:** Proposed  
**Platform:** Web Application  
**Framework:** Next.js + TypeScript  
**Project Type:** Personal Learning / Developer Tool

---

## 1. Product Overview

### 1.1 Product Description

ReqLab is a web-based HTTP request inspection tool that allows developers to construct, execute, and analyze HTTP requests through a simple interface.

Inspired by tools such as Postman and Insomnia, ReqLab focuses on core HTTP functionality, response inspection, and request history without the complexity of full-scale API development platforms.

The project prioritizes hands-on software engineering, native browser and server APIs, and a lightweight architecture.

### 1.2 Problem Statement

Developers frequently interact with REST APIs during development and debugging. Although existing tools offer extensive functionality, they can introduce unnecessary complexity for basic HTTP inspection.

Additionally, relying entirely on existing abstractions can limit opportunities to understand request execution, HTTP protocols, error handling, and response processing.

ReqLab addresses this by providing a focused environment for constructing HTTP requests and examining their results.

### 1.3 Product Objectives

- Allow developers to construct and execute HTTP requests.
- Display comprehensive HTTP response information.
- Support custom request headers and request bodies.
- Persist request history locally.
- Provide clear diagnostics for network and HTTP errors.
- Enable comparison of HTTP responses.
- Maintain a simple, responsive interface.

### 1.4 Target Users

- Software developers
- Computer Science students
- Backend and full-stack developers
- Developers learning REST APIs and HTTP

---

## 2. Project Scope

### 2.1 In Scope

- HTTP request construction and execution
- GET, POST, PUT, PATCH, and DELETE methods
- Custom request headers
- Query parameter management
- JSON and plain-text request bodies
- Response body and header inspection
- HTTP status and response-time measurement
- Request history
- Saved request collections
- Response comparison
- Configurable request timeouts
- Local persistence

### 2.2 Out of Scope

- User authentication
- Cloud synchronization
- Team collaboration
- API documentation generation
- GraphQL-specific tooling
- WebSocket inspection
- Automated API testing suites
- AI-generated API requests
- Browser extensions
- Enterprise access controls

---

## 3. Functional Requirements

### 3.1 Request Builder

**FR-01:** The system shall allow users to enter and edit HTTP request URLs.

**FR-02:** The system shall support GET, POST, PUT, PATCH, and DELETE methods.

**FR-03:** The system shall allow users to add, modify, enable, disable, and remove custom HTTP request headers.

**FR-04:** The system shall allow users to manage query parameters through a key-value interface.

**FR-05:** The system shall support JSON and plain-text request bodies for applicable methods.

**FR-06:** The system shall validate URL syntax before executing a request.

**FR-07:** The system shall allow users to configure a request timeout.

**FR-08:** The system shall prevent duplicate execution of the same request through repeated Send clicks while that request is pending.

### 3.2 HTTP Request Execution

**FR-09:** The system shall execute requests using the native Fetch API.

**FR-10:** The system shall support browser-based requests and an optional server-side proxy for APIs restricted by browser CORS policies.

**FR-11:** The system shall provide a loading indicator while a request is being processed.

**FR-12:** The system shall allow users to cancel an ongoing request using AbortController.

**FR-13:** The system shall distinguish HTTP error responses from network-level failures.

**FR-14:** The system shall record elapsed request time using a high-resolution timer.

**FR-15:** The server-side proxy shall restrict unauthorized destinations, block private and internal network targets, and validate outbound redirects to mitigate SSRF vulnerabilities.

### 3.3 Response Inspector

**FR-16:** The system shall display the HTTP status code and status text when available.

**FR-17:** The system shall display the elapsed request time in milliseconds.

**FR-18:** The system shall display the response body size in bytes or appropriate units.

**FR-19:** The system shall display response headers as key-value pairs.

**FR-20:** The system shall format valid JSON responses for readability.

**FR-21:** The system shall support viewing response bodies as formatted text or raw text.

**FR-22:** The system shall allow users to copy response content to the clipboard.

**FR-23:** The system shall display appropriate messages for empty responses, malformed JSON, and unsupported content types.

**FR-24:** The system shall differentiate HTTP success responses, client errors, and server errors through visual indicators.

### 3.4 Request History

**FR-25:** The system shall automatically store completed request attempts in a local history.

**FR-26:** The system shall display historical requests with their method, URL, timestamp, and outcome.

**FR-27:** The system shall allow users to select and replay historical requests.

**FR-28:** The system shall allow users to delete individual history entries.

**FR-29:** The system shall allow users to clear request history.

**FR-30:** The system shall persist request history across browser sessions using IndexedDB.

### 3.5 Request Collections

**FR-31:** The system shall allow users to create named request collections.

**FR-32:** The system shall allow users to save configured HTTP requests into collections.

**FR-33:** The system shall allow users to edit and delete saved requests.

**FR-34:** The system shall allow users to organize saved requests within collections.

### 3.6 Response Comparison

**FR-35:** The system shall allow users to select two stored responses for comparison.

**FR-36:** The system shall display differences between JSON response structures and values.

**FR-37:** The system shall highlight added, removed, and modified fields.

**FR-38:** The system shall display differences in HTTP status codes and response times.

---

## 4. Non-Functional Requirements

| Category | Requirement |
|---|---|
| Performance | The interface should remain responsive during request execution. |
| Usability | Common operations should be accessible without navigating multiple pages. |
| Responsiveness | The application should support desktop and tablet screens. |
| Reliability | Network failures and malformed responses must not crash the application. |
| Security | The proxy must enforce outbound network restrictions and request limits. |
| Privacy | Requests and history should remain local unless explicitly sent to a target API. |
| Maintainability | Request execution, response processing, and persistence should be separate modules. |
| Accessibility | Interactive controls should support keyboard navigation and accessible labels. |

**Measurement clarification:** Response time measures elapsed time observed by the application, not pure network latency. Response size is based on received body bytes and may differ from transferred bytes.

---

## 5. User Interface Requirements

### 5.1 Main Workspace

The application shall use a developer-oriented workspace consisting of three primary regions:

**Left Sidebar**
- Request history
- Saved collections
- Saved requests

**Request Workspace**
- HTTP method selector
- URL input
- Send and Cancel controls
- Query Parameters tab
- Headers tab
- Body tab

**Response Workspace**
- HTTP status
- Response time
- Response size
- Body tab
- Headers tab
- Raw response tab

### 5.2 Interface States

The interface shall account for:

- Empty workspace
- Request configuration
- Request loading
- Successful HTTP response
- HTTP 4xx or 5xx response
- Network failure
- Request timeout
- Request cancellation
- Invalid URL
- Empty response body

---

## 6. Technical Architecture

### 6.1 Technology Stack

| Layer | Technology |
|---|---|
| Frontend | Next.js App Router |
| Language | TypeScript |
| Styling | Tailwind CSS |
| UI Components | shadcn/ui |
| Request Execution | Native Fetch API |
| Request Cancellation | AbortController |
| Persistence | IndexedDB |
| JSON Editor | Monaco Editor (optional enhancement) |
| Server Proxy | Next.js Route Handlers |
| Package Manager | pnpm |

### 6.2 High-Level Architecture

**Browser Client**
- Request Builder
- Request Execution Controller
- Response Inspector
- History and Collection Manager
- Local Persistence Layer

**Execution Modes**

1. **Direct Mode:** The browser directly sends HTTP requests to the target API. Browser security and CORS restrictions apply.
2. **Proxy Mode:** A restricted Next.js Route Handler forwards requests to explicitly permitted public destinations and returns the inspected response.

### 6.3 Data Models

**HttpRequest**
- id
- method
- url
- headers
- queryParams
- body
- bodyType
- timeoutMs
- executionMode
- createdAt

**HttpResponse**
- id
- requestId
- status
- statusText
- headers
- body
- bodySizeBytes
- durationMs
- receivedAt
- errorType

**RequestCollection**
- id
- name
- description
- requestIds
- createdAt
- updatedAt

**RequestHistory**
- id
- requestSnapshot
- responseSnapshot
- executedAt

Request and response snapshots may contain sensitive data, so the application must provide clear local-storage behavior, deletion controls, and avoid persisting secrets by default.

---

## 7. Development Milestones

### Phase 1 — Core HTTP Inspector (MVP)

**Goal:** Build a functional HTTP request client.

- Set up Next.js and TypeScript.
- Implement URL input and method selector.
- Implement browser-based Fetch API execution.
- Display HTTP status, response time, and body.
- Display response headers.
- Implement basic JSON formatting.
- Implement loading and error states.

**Acceptance Criteria:** Users can send a CORS-permitted GET request and inspect the returned response accurately.

### Phase 2 — Advanced Request Construction

**Goal:** Support practical REST API testing.

- Implement custom headers.
- Implement query parameter management.
- Implement JSON and text request bodies.
- Support POST, PUT, PATCH, and DELETE.
- Implement timeout handling.
- Implement request cancellation.

**Acceptance Criteria:** Users can configure and execute multiple HTTP methods with custom headers, parameters, and appropriate bodies.

### Phase 3 — Local Persistence

**Goal:** Make previous requests reusable.

- Integrate IndexedDB.
- Implement request history.
- Implement request replay.
- Implement saved collections.
- Implement history deletion and clearing.

**Acceptance Criteria:** Users can reopen ReqLab and retrieve their locally saved, non-sensitive requests.

### Phase 4 — Advanced Inspection

**Goal:** Improve diagnostics and debugging.

- Implement response comparison.
- Add structured JSON difference highlighting.
- Improve raw response viewing.
- Add response copying.
- Add restricted server-side proxy execution.
- Implement proxy security tests.

**Acceptance Criteria:** Users can compare responses and safely execute proxy requests against allowed public API hosts.

### Phase 5 — Polish and Release

**Goal:** Deliver a stable developer tool.

- Improve responsive layouts.
- Implement keyboard shortcuts.
- Add accessible interaction states.
- Add unit and integration tests.
- Optimize handling of large responses.
- Document configuration and known limitations.

**Acceptance Criteria:** Core workflows pass automated tests and manual verification without critical defects.

---

## 8. Success Criteria

| Metric | Target |
|---|---|
| Supported HTTP methods | 5 |
| Request execution correctness | 100% on defined test cases |
| Response status accuracy | 100% on defined test cases |
| Request cancellation | Successfully aborts pending supported requests |
| History persistence | Survives page reloads |
| JSON formatting | Handles valid, invalid, and empty JSON safely |
| Error handling | All defined failure scenarios produce clear feedback |
| Proxy safety | All defined SSRF and unauthorized-host test cases rejected |

---

## 9. Development Principles

This project is intended to rebuild hands-on engineering skills and understanding of web technologies.

- Implement core logic manually before introducing abstractions.
- Prefer native JavaScript and browser APIs.
- Use third-party libraries only when they remove unnecessary UI complexity.
- Keep the first milestone small and independently functional.
- Write tests for HTTP behavior and edge cases.
- Prioritize understanding over implementation speed.
- Avoid unnecessary features, external services, and premature optimization.
- Use AI for conceptual guidance and code review rather than generating the entire implementation.

---

## 10. Definition of Done

ReqLab is considered complete when a developer can construct an HTTP request, execute it, inspect its response, diagnose failures, save reusable requests locally, and compare previous responses through a stable Next.js interface.

The product must achieve these capabilities without requiring authentication, cloud storage, or third-party API testing services.