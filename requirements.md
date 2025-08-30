# Airbnb Clone Backend Requirements

This document specifies the technical and functional requirements for the backend features of the Airbnb Clone project.

---

## 1. User Authentication

**Purpose:** Allow users to register, login, and manage their profiles securely.

**API Endpoints:**

| Method | Endpoint           | Description           | Input Example                                     | Output Example |
|--------|------------------|---------------------|-------------------------------------------------|----------------|
| POST   | /api/auth/register | Register a new user  | `{ "username": "john", "email": "john@mail.com", "password": "12345678" }` | `{ "message": "User registered", "token": "<JWT>", "user": { "id":1 } }` |
| POST   | /api/auth/login    | Login user           | `{ "email":"john@mail.com", "password":"12345678" }` | `{ "message": "Login successful", "token": "<JWT>", "user": { "id":1 } }` |
| GET    | /api/auth/profile  | Get profile          | -                                               | `{ "id":1, "username":"john", "email":"john@mail.com" }` |

**Validation Rules:**
- Email must be unique and valid.
- Password: minimum 8 characters, letters + numbers.
- Username must be unique.

**Performance:** Authentication requests should respond within 500ms.

---

## 2. Property Management

**Purpose:** Enable hosts to add, edit, delete, and view property listings.

**API Endpoints:**

| Method | Endpoint               | Description        | Input Example                                   | Output Example |
|--------|----------------------|------------------|-----------------------------------------------|----------------|
| POST   | /api/properties        | Add property      | `{ "title":"Sea View Apartment", "location":"Miami", "price":150 }` | `{ "message":"Property added", "property": { "id":1 } }` |
| GET    | /api/properties        | List properties   | -                                             | `[ { "id":1, "title":"Sea View Apartment" } ]` |
| GET    | /api/properties/:id    | Fetch property    | -                                             | `{ "id":1, "title":"Sea View Apartment", "price":150 }` |
| PUT    | /api/properties/:id    | Update property   | `{ "price":180 }`                              | `{ "message":"Property updated" }` |
| DELETE | /api/properties/:id    | Delete property   | -                                             | `{ "message":"Property deleted" }` |

**Validation Rules:**
- Price must be a positive number.
- Title, description, and location are required.
- Images must be valid file types.

**Performance:** Property retrieval should support 1000+ records without timeout.

---

## 3. Booking System

**Purpose:** Manage booking requests, cancellations, and status updates.

**API Endpoints:**

| Method | Endpoint                  | Description         | Input Example                                           | Output Example |
|--------|---------------------------|-------------------|--------------------------------------------------------|----------------|
| POST   | /api/bookings             | Create booking     | `{ "propertyId":1, "userId":2, "startDate":"2025-09-05", "endDate":"2025-09-10" }` | `{ "message":"Booking confirmed", "booking": { "id":1, "status":"confirmed" } }` |
| GET    | /api/bookings/:userId     | Get user bookings  | -                                                      | `[ { "id":1, "propertyId":1, "status":"confirmed" } ]` |
| PUT    | /api/bookings/:id/cancel  | Cancel booking     | -                                                      | `{ "message":"Booking canceled" }` |

**Validation Rules:**
- Start date must be before end date.
- Booking dates must not overlap with existing bookings.
- Property and user must exist.

**Performance:** Booking creation should respond within 1 second.

---

## 4. Payment Integration

**Purpose:** Handle secure payments for bookings.

**API Endpoints:**

| Method | Endpoint            | Description                | Input Example                       | Output Example |
|--------|-------------------|----------------------------|------------------------------------|----------------|
| POST   | /api/payments      | Process payment            | `{ "bookingId":1, "amount":150 }` | `{ "message":"Payment successful", "status":"paid" }` |
| GET    | /api/payments/:id  | Fetch payment details      | -                                  | `{ "bookingId":1, "status":"paid" }` |

**Validation Rules:**
- Payment amount must match booking price.
- Payment information must be securely encrypted.

**Performance:** Payment processing should respond within 2 seconds.

---

## 5. Reviews and Ratings

**Purpose:** Allow guests to leave reviews and hosts to respond.

**API Endpoints:**

| Method | Endpoint           | Description             | Input Example                          | Output Example |
|--------|------------------|-----------------------|---------------------------------------|----------------|
| POST   | /api/reviews      | Add a review          | `{ "bookingId":1, "rating":5, "comment":"Great place!" }` | `{ "message":"Review submitted" }` |
| GET    | /api/reviews/:propertyId | Get property reviews | -                                     | `[ { "userId":2, "rating":5, "comment":"Great place!" } ]` |

**Validation Rules:**
- One review per booking.
- Rating must be between 1 and 5.

---

## 6. Notifications System

**Purpose:** Notify users of bookings, cancellations, and payments via email or in-app messages.

**Implementation:**
- Use SendGrid or Mailgun for email notifications.
- Implement in-app notifications for key events.

---

## 7. Admin Dashboard

**Purpose:** Allow administrators to monitor and manage users, listings, bookings, and payments.

**Features:**
- View all users, properties, bookings, and payments.
- Manage flagged content or disputes.

---

## Non-Functional Requirements

1. **Scalability:** Modular architecture, horizontal scaling, load balancers.  
2. **Security:** Encrypt sensitive data (passwords, pay

