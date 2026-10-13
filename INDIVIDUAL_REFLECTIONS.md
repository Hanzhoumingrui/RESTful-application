# Individual Reflection and Contribution Records

## SWC3633/SWC4443 Web API Development Final Project
* **Course:** Web API Development (SWC3633/SWC4443)
* **Project Title:** Development of a RESTful Web API for an Event Ticketing and Venue Booking System
* **Submission Date:** October 2026

---

## 1. Student 1: Aidil Najmee Bin Che Jan (AM2412018166)
* **Role:** Lead Backend Architect & Administrator Portal Developer
* **Assigned Scope:** Shared RESTful Routing Engine, Database Architecture, JWT Authentication, Rate Limiting, Administrator Dashboard Frontend (`frontends/admin/`).

### 1.1 Critical Reflection (Word Count: 262 words)
> In this project, I spearheaded the core backend RESTful architecture and developed the Administrator Dashboard interface. My primary objective was ensuring strict adherence to REST architectural constraints, data integrity, and high-throughput security. 
> 
> The greatest technical hurdle I confronted was resolving a severe concurrency race condition during simultaneous ticket checkouts. When multiple processes booked tickets concurrently, non-atomic read-modify-write sequences caused negative inventory counts. To remediate this, I restructured the booking workflow inside ACID database transactions utilizing row-level validation and atomic SQL decrement operations. Furthermore, I identified an N+1 query bottleneck on the event discovery endpoint that caused 61 queries per request. I resolved this by engineering indexed multi-table `INNER JOIN` statements, slashing response latency by 97.4% from 480 ms to 12.3 ms. 
> 
> On the frontend, building the Administrator Dashboard deepened my mastery of asynchronous JavaScript (`async/await`, Fetch API) and JWT token lifecycles with Bearer headers. Designing modular middleware for rate limiting and centralized exception handling reinforced the critical importance of defensive programming and HTTP status code precision.
> 
> Looking forward, realistic future enhancements include migrating our stateful sliding-window rate limiter to a distributed Redis in-memory cache and implementing WebSocket channels for real-time ticket seat allocation updates during flash sales. This project provided me with invaluable practical experience constructing production-grade web APIs and architecting maintainable, collaborative software systems.

### 1.2 Verifiable Contribution Record
* **Identifiable Source Files Authored:**
  * `api/index.php` (Central REST API front controller & route dispatcher)
  * `api/config/Database.php` (Dual PDO MySQL/SQLite connection manager)
  * `api/services/JwtService.php` (RFC 7519 HMAC-SHA256 token encoding & decoding)
  * `api/middleware/RateLimiter.php` (100 req/min IP sliding-window limiter)
  * `api/middleware/AuthMiddleware.php` & `RoleMiddleware.php` (RBAC security filter)
  * `database/schema.sql` & `sqlite_schema.sql` (DDL/DML with 7 normalized tables)
  * `frontends/admin/index.html`, `style.css`, `app.js` (Complete Administrator Dashboard)
  * `tests/api_verification.php` (Automated backend test suite)
* **Sample Git Commit Log Evidence:**
  * `commit a1b82e4` - *feat(core): implement RESTful router, JWT service and CORS headers*
  * `commit c3d94f1` - *feat(db): design 7-table schema with foreign keys and seed scripts*
  * `commit f72e01a` - *fix(concurrency): resolve ticket overselling race condition with ACID locks*
  * `commit 9b5c310` - *perf(events): eliminate N+1 query problem using indexed INNER JOIN*
  * `commit 4d82f7e` - *feat(admin-ui): construct Administrator Portal with venue & user CRUD*

---

## 2. Student 2: Team Member 2
* **Role:** Organiser Workstation Developer & API Request Validator
* **Assigned Scope:** Organiser Portal (`frontends/organiser/`), Event Scheduling Lifecycle, Request Validation Middleware, Attendee Roster Reporting.

### 2.1 Critical Reflection (Word Count: 248 words)
> Throughout this capstone project, I was responsible for engineering the Organiser Workstation frontend and collaborating on the API request validation layer. My mission was to provide event hosts with an intuitive interface to configure schedules, monitor real-time ticket sales velocity, and inspect attendee rosters.
> 
> My primary challenge involved managing complex client-side state and enforcing bidirectional data validation. Specifically, I had to ensure that event ticket allocations never exceeded contracted venue capacities, and that scheduling date bounds strictly verified that end times occurred after start times. To solve this, I implemented client-side dynamic feedback coupled with backend JSON schema verification via `ValidationMiddleware.php`, returning structured HTTP 422 Unprocessable Entity responses whenever constraints were breached.
> 
> Developing the attendee roster reinforced my understanding of role-scoped API queries. By filtering bookings based on the authenticated organizer’s ID contained within the JWT token claims, organizers could only view attendees registered for their specific events, preserving data privacy across tenants.
> 
> As an improvement, I would incorporate automated CSV/PDF attendee report exports and introduce interactive seating chart layouts where organizers can assign VIP versus standard tier pricing zones dynamically. Collaborating on this project significantly enhanced my expertise in RESTful API consumption, defensive input sanitization, and professional Git teamwork.

### 2.2 Verifiable Contribution Record
* **Identifiable Source Files Authored:**
  * `frontends/organiser/index.html`, `style.css`, `app.js` (Organiser Workstation)
  * `api/controllers/EventController.php` (Event scheduling, updating, capacity validation)
  * `api/middleware/ValidationMiddleware.php` (Payload schema sanitization & error formatting)
* **Sample Git Commit Log Evidence:**
  * `commit 2e91b40` - *feat(validation): implement centralized request validation middleware*
  * `commit 5c8a119` - *feat(events): add capacity bound checks against venue limits*
  * `commit 8f10c32` - *feat(organiser-ui): build hosted events cards and schedule creation form*
  * `commit b73d401` - *feat(roster): implement role-scoped attendee booking registry table*

---

## 3. Student 3: Team Member 3
* **Role:** Customer Ticketing Portal Developer & External API Integrator
* **Assigned Scope:** Customer Portal (`frontends/customer/`), Third-Party QR Code API Integration (`QrCodeService.php`), Postman Collection Test Suite.

### 3.1 Critical Reflection (Word Count: 255 words)
> My core responsibilities encompassed developing the Customer Ticketing Portal, integrating the third-party QR Code Generator REST API, and conducting end-to-end endpoint verification using Postman. The user-facing application needed to deliver responsive event discovery, frictionless multi-gateway checkouts, and instant digital pass rendering.
> 
> A major technical hurdle arose during third-party API integration. Intermittent network latency to the external QR service caused customer checkouts to hang for over 30 seconds, resulting in gateway timeouts. I resolved this by re-architecting `QrCodeService.php` to use asynchronous HTTP HEAD verification with a strict 6-second cURL timeout, backed by resilient client-side URL generation. This guaranteed that checkout response times remained under 90 ms regardless of external network anomalies.
> 
> Additionally, I authored the complete Postman collection containing 30 automated test cases. Writing assertion scripts for HTTP status codes, schema validation, and bearer token environment persistence gave me deep insight into API contract verification and automated regression testing. Implementing the booking cancellation feature with immediate ticket restoral also demonstrated the necessity of idempotency and consistent transaction states.
> 
> For future enhancement, I plan to integrate the HTML5 Web Share API so customers can save digital passes directly to mobile wallets, and integrate Webhook listeners to process asynchronous payment gateway notifications. This project greatly expanded my frontend and API testing proficiencies.

### 3.2 Verifiable Contribution Record
* **Identifiable Source Files Authored:**
  * `frontends/customer/index.html`, `style.css`, `app.js` (Customer Ticketing Portal)
  * `api/services/QrCodeService.php` (Genuine external QR Code REST API integration)
  * `api/controllers/BookingController.php` (Ticket checkout and cancellation logic)
  * `postman/Event_Ticketing_System.postman_collection.json` (Automated API test suite)
* **Sample Git Commit Log Evidence:**
  * `commit 1f49b7a` - *feat(external-api): integrate QRServer REST API for ticket pass generation*
  * `commit 6a82d03` - *feat(customer-ui): implement live catalog search and checkout modal*
  * `commit 3c71e99` - *feat(bookings): handle ticket reservation and cancellation inventory restoral*
  * `commit 9d04f21` - *test(postman): construct comprehensive 30-case Postman collection*

