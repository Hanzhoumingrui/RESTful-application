# Combined Group Video Demonstration Script & Storyboard

## SWC3633/SWC4443 Web API Development Final Project
* **Project Title:** Development of a RESTful Web API for an Event Ticketing and Venue Booking System
* **Institution:** Universiti Poly-Tech Malaysia (UPTM)
* **Team Members:**
  * **Student 1 (Team Leader):** Aidil Najmee Bin Che Jan (Student ID: AM2412018166) - *Backend Lead & Administrator Frontend*
  * **Student 2:** Team Member 2 - *Organiser / Staff Portal Frontend*
  * **Student 3:** Team Member 3 - *Customer Ticketing Portal Frontend*
* **Video Platform:** YouTube (Unlisted / Public Link)
* **Total Video Duration:** ~12 to 14 minutes (Intro/Outro + 3 to 4 minutes per member)

---

## Complete Video Timeline Overview

| Timestamp | Section | Presenter | Key Focus & Rubric Alignment |
| :--- | :--- | :--- | :--- |
| **00:00 - 01:30** | System Overview & Architecture | Aidil Najmee (Group Intro) | Objectives, 3-tier architecture, tech stack, ERD, and endpoints |
| **01:30 - 05:30** | Student 1 Individual Presentation | **Aidil Najmee (AM2412018166)** | Admin Dashboard: Venue CRUD, User RBAC, live audit logs, error handling |
| **05:30 - 09:15** | Student 2 Individual Presentation | **Student 2** | Organiser Portal: Event scheduling, capacity bounds, attendee roster |
| **09:15 - 13:00** | Student 3 Individual Presentation | **Student 3** | Customer Portal: Catalog search, checkout, genuine QR API, cancellation |
| **13:00 - 14:00** | Group Conclusion & Summary | All Members | Verification checklist, GitHub repo links, CLO3 alignment |

---

## Detailed Section-by-Section Script

### 1. Group Introduction & System Architecture (00:00 - 01:30)
**Presenter:** Aidil Najmee Bin Che Jan (AM2412018166)  
**Visual on Screen:** Title Slide displaying Student Names, IDs, Section, followed by System Architecture Mermaid Diagram.

> **Aidil Najmee:**  
> "Assalamu’alaikum and good day to our lecturer and evaluators. Welcome to our final project presentation for **SWC3633/SWC4443 Web API Development**.  
>  
> Today, our team is proud to present the **EventHub Ticketing and Venue Booking System**. Our system was engineered to fulfill Course Learning Outcome 3 (CLO3) by constructing a production-grade, secure, and scalable RESTful application.  
>  
> Architecturally, our system is structured into a clean three-tier model:
> 1. At the core, we engineered a shared RESTful API written in PHP 8 with modular routing, JWT authentication (HMAC-SHA256), sliding-window rate limiting, and centralized error handling.
> 2. The persistence layer utilizes a normalized MySQL relational database consisting of seven related tables with primary keys, foreign key constraints, and performance indexes, with seamless zero-config SQLite dual-driver fallback.
> 3. For the presentation layer, each of us has developed a completely independent frontend application tailored to a specific user role: an **Administrator Dashboard**, an **Organiser Portal**, and a **Customer Ticketing Portal**.
> 4. We also integrated a genuine third-party external API service—the QRServer REST API—to generate authentic digital ticket verification entry passes.  
>  
> Without further ado, let us begin the individual demonstrations."

---

### 2. Student 1 Individual Presentation: Administrator Portal (01:30 - 05:30)
**Presenter:** Aidil Najmee Bin Che Jan  
**Student ID Banner on Screen:** `AIDIL NAJMEE BIN CHE JAN | AM2412018166 | SECTION 3`  
**Application Shown:** `frontends/admin/index.html` + Network Tab / Postman

> **Aidil Najmee:**  
> "Greetings, I am **Aidil Najmee Bin Che Jan, Student ID AM2412018166**. As the backend lead and author of the **Administrator Frontend**, I will demonstrate our core REST API, security mechanisms, and the administrative dashboard.  
>  
> **[Action: Log into Admin Portal & Show Dashboard KPIs]**  
> Here, upon logging in, my frontend sends a `POST /api/v1/auth/login` request. The API authenticates the credentials using `password_verify()` and returns a signed JWT containing my user ID and `'admin'` role claim. This token is securely stored and injected into the `Authorization: Bearer <token>` header for all subsequent calls.  
>  
> On our executive dashboard, the frontend communicates with `GET /api/v1/stats`. It displays real-time gross revenue, total tickets sold, active events, and venue counts, populated via fast SQL aggregate queries.  
>  
> **[Action: Navigate to Venues Management & Perform CRUD]**  
> Let us look at the **Venues Resource (`/venues`)**:
> - Clicking `+ Add New Venue`, I will register *'Sunway Pyramid Convention Centre'* with a capacity of 2,500 seats and hourly rate of RM 750.  
> - Upon submission, a `POST /api/v1/venues` is fired. Notice the instant toast notification and the new record appearing dynamically in the table.  
> - Let's edit this venue to adjust its capacity to 2,800. This triggers a `PUT /api/v1/venues/6`.  
> - Now, let's demonstrate business rule validation and foreign key integrity. If I try to delete Venue ID 1 (*KLCC*), which already has scheduled events, our backend error handling middleware intercepts the request and responds with **HTTP 409 Conflict**: *'Cannot delete venue because events are currently attached to it'*.  
>  
> **[Action: Navigate to User Accounts & Audit Logs]**  
> In the User Accounts section, the admin can query `GET /api/v1/users` with role filters. Normal customers or organisers attempting to call this endpoint receive **HTTP 403 Forbidden**, proving our Role-Based Access Control (RBAC) middleware is working properly.  
>  
> Finally, looking at our **Live API Audit Trail (`api_logs`)**, our middleware measures request latency via microsecond timestamps, recording every method, status code, and IP address.  
>  
> That concludes my individual demonstration. I will now hand over the presentation to Student 2 to showcase the Organiser Portal."

---

### 3. Student 2 Individual Presentation: Organiser Portal (05:30 - 09:15)
**Presenter:** Student 2 (Co-Member)  
**Student ID Banner on Screen:** `[STUDENT 2 NAME] | [STUDENT 2 ID]`  
**Application Shown:** `frontends/organiser/index.html`

> **Student 2:**  
> "Thank you, Aidil. Good day everyone, I am **[Student 2 Name], Student ID [Student 2 ID]**. I am responsible for the **Organiser Portal** and event scheduling lifecycle.  
>  
> **[Action: Log in as Organiser Sarah Tan]**  
> Logging in as Sarah Tan, the JWT payload identifies my role as `'organiser'`.  
>  
> On my hosted events dashboard, my frontend calls `GET /api/v1/events` with parameter scoping. For each event, we calculate real-time ticket sales velocity: showing total allocated tickets, remaining tickets, and visual progress bars.  
>  
> **[Action: Schedule New Event]**  
> Let us schedule a new conference:
> - Title: *'ASEAN Cyber Resilience Forum 2026'*  
> - Category: *'Technology & AI'*  
> - Venue: Selecting *'Kuala Lumpur Convention Centre'* dynamically updates the form's capacity hint to 3,000 seats.  
> - Dates: If I intentionally set the end date prior to the start date, our request validation middleware instantly blocks the submission with **HTTP 422 Unprocessable Entity**: *'Event end_time must be strictly after start_time'*.  
> - Correcting the dates and setting 400 tickets at RM 180 per ticket, the `POST /api/v1/events` succeeds with **HTTP 201 Created**.  
>  
> **[Action: Attendee Bookings Roster]**  
> Moving to the Attendee Bookings Roster tab, `GET /api/v1/bookings` retrieves only bookings associated with my hosted events. Clicking *'View QR Pass'* on any attendee previews their digital QR code ticket generated via the external API.  
>  
> I will now hand the presentation over to Student 3 for the Customer Ticketing Portal."

---

### 4. Student 3 Individual Presentation: Customer Portal (09:15 - 13:00)
**Presenter:** Student 3 (Co-Member)  
**Student ID Banner on Screen:** `[STUDENT 3 NAME] | [STUDENT 3 ID]`  
**Application Shown:** `frontends/customer/index.html`

> **Student 3:**  
> "Thank you. Hello everyone, I am **[Student 3 Name], Student ID [Student 3 ID]**. I developed the **Customer Ticketing Portal**, focusing on event discovery, responsive UI/UX, checkout workflows, and genuine external API integration.  
>  
> **[Action: Browse Event Catalog with Live Search]**  
> On the public catalog, customers can search for events. As I type *'Summit'* into the search bar, a 300ms debounce fires `GET /api/v1/events?search=Summit` without reloading the page.  
> Clicking category pills like *'Live Music'* or changing the sort dropdown to *'Price: Low to High'* updates the URL parameters and re-renders the grid seamlessly.  
>  
> **[Action: Reserve Tickets & Checkout]**  
> Let us book tickets for the *'Cloud Native & API Summit'*:
> - Clicking *'Reserve Seat'* opens our checkout modal.  
> - I select 2 tickets. Notice the live total price automatically updates to RM 240.00.  
> - I choose *'Online Banking (FPX)'* and click *'Confirm & Generate QR Pass'*.  
>  
> **[Action: Inspect Network Tab & Show External QR API Call]**  
> Behind the scenes, the backend executes `POST /api/v1/bookings`. It decrements available tickets within an ACID transaction, and invokes the **genuine external QR Code API service** at `api.qrserver.com` via cURL.  
>  
> The API returns **HTTP 201 Created** containing our unique booking reference (`BK-2026-F41A92B1`) and the generated QR code URL. The modal instantly renders the verifiable QR ticket pass on screen!  
>  
> **[Action: Demonstrate My Bookings & Booking Cancellation]**  
> Navigating to *'My Bookings'*, the reservation appears in the customer's history.  
> If the customer cancels this booking, clicking *'Cancel Booking'* triggers `DELETE /api/v1/bookings/{id}`. The status changes to `'cancelled'`, and the 2 tickets are immediately restored back to the event's available ticket inventory.  
>  
> I will now hand back to Aidil for the group conclusion."

---

### 5. Group Conclusion & Submission Wrap-Up (13:00 - 14:00)
**Presenter:** Aidil Najmee (and all group members)  
**Visual on Screen:** GitHub Repository structure, Postman Collection runner, and project summary slide.

> **Aidil Najmee:**  
> "To conclude our presentation:  
> - All required core endpoints across `/users`, `/venues`, `/events`, and `/bookings` are fully functional with reliable CRUD operations and strict RESTful conventions.  
> - Security features including JWT tokens, role-based access control, sliding-window rate limiting, and input sanitization have been rigorously verified.  
> - Our complete source code, executable SQL scripts (both MySQL and SQLite), Postman collections with automated tests, and individual technical evidence portfolios are available on GitHub.  
>  
> Thank you very much for your time and guidance throughout this semester!"

