# Airbnb Clone – Backend Requirements Specification

This document outlines the technical and functional requirements for three key backend features: User Authentication, Property Management, and Booking System.

---

## 1. User Authentication

### Functional Requirements
- Users must be able to register, log in, and manage their profiles.
- Roles include: guest, host, admin.
- Passwords must be securely hashed.

### API Endpoints
- `POST /api/register`
  - **Input**: `{ first_name, last_name, email, password, role }`
  - **Output**: `201 Created` with user ID or error message
- `POST /api/login`
  - **Input**: `{ email, password }`
  - **Output**: `200 OK` with JWT token or `401 Unauthorized`
- `GET /api/profile/:id`
  - **Output**: User profile data
- `PUT /api/profile/:id`
  - **Input**: `{ first_name?, last_name?, password? }`
  - **Output**: `200 OK` or validation error

### Validation Rules
- Email must be unique and valid format.
- Password must be at least 8 characters.
- Role must be one of: guest, host, admin.

### Performance Criteria
- Login response time < 300ms.
- Registration must complete within 500ms under normal load.

---

## 2. Property Management

### Functional Requirements
- Hosts can list, edit, and delete properties.
- Guests can search and filter properties.

### API Endpoints
- `POST /api/properties`
  - **Input**: `{ host_id, name, description, location, pricepernight }`
  - **Output**: `201 Created` with property ID
- `GET /api/properties`
  - **Query Params**: `location, price_min, price_max`
  - **Output**: List of matching properties
- `PUT /api/properties/:id`
  - **Input**: `{ name?, description?, pricepernight? }`
  - **Output**: `200 OK` or error
- `DELETE /api/properties/:id`
  - **Output**: `204 No Content`

### Validation Rules
- Price must be a positive number.
- Location must be a non-empty string.
- Host ID must exist in the user table.

### Performance Criteria
- Search results must return within 400ms.
- Listing creation must complete within 600ms.

---

## 3. Booking System

### Functional Requirements
- Guests can book available properties.
- Hosts can confirm or cancel bookings.
- Booking status: pending, confirmed, cancelled.

### API Endpoints
- `POST /api/bookings`
  - **Input**: `{ property_id, user_id, start_date, end_date }`
  - **Output**: `201 Created` with booking ID
- `GET /api/bookings/:id`
  - **Output**: Booking details
- `PUT /api/bookings/:id/status`
  - **Input**: `{ status }`
  - **Output**: `200 OK` or error

### Validation Rules
- Dates must be valid and not overlap with existing bookings.
- Total price must be calculated based on date range and nightly rate.
- Status must be one of: pending, confirmed, cancelled.

### Performance Criteria
- Booking creation must complete within 700ms.
- Status updates must reflect in real-time.

---

## Notes
- All endpoints must return standardized error messages.
- Authentication required for protected routes using JWT.
- Rate limiting and input sanitization must be enforced.

