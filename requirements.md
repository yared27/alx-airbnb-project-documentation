# Backend Requirement Specifications - ALX Airbnb Project

This document outlines the functional and technical requirements for the core backend features.

---

## 1. User Authentication

### 🔹 Functional Requirements
- Users can register using email, password, and profile details.
- Users can log in and receive a JWT access token.
- Authentication is required for protected endpoints.

### 🔹 API Endpoints

#### POST /api/register
- **Input**: `{ "email": "user@example.com", "password": "pass1234", "first_name": "John", "last_name": "Doe" }`
- **Validation**:
  - Email must be valid and unique.
  - Password must be at least 6 characters.
- **Output**: `{ "token": "JWT_TOKEN", "user_id": "uuid" }`

#### POST /api/login
- **Input**: `{ "email": "user@example.com", "password": "pass1234" }`
- **Output**: `{ "token": "JWT_TOKEN", "user_id": "uuid" }`
- **Errors**: 401 if invalid credentials

### 🔹 Performance
- Response time: ≤ 500ms
- Passwords stored with bcrypt hashing

---

## 2. Property Management

### 🔹 Functional Requirements
- Hosts can create, update, view, and delete their property listings.
- Properties contain name, description, location, price per night, and availability.

### 🔹 API Endpoints

#### POST /api/properties
- **Input**: `{ "name": "Cozy Room", "location": "Addis Ababa", "description": "Clean and quiet", "pricepernight": 35.00 }`
- **Validation**:
  - All fields required.
  - Price must be a positive decimal.
- **Authentication**: Required (host role)
- **Output**: Property ID and success message

#### GET /api/properties/:id
- **Output**: JSON of property details

#### PUT /api/properties/:id
- **Authentication**: Only the property owner can update
- **Validation**: Same as POST

#### DELETE /api/properties/:id
- **Authorization**: Owner only

### 🔹 Performance
- Queries should use indexed `property_id` and `host_id`
- Max response time: ≤ 800ms

---

## 3. Booking System

### 🔹 Functional Requirements
- Users can book available properties for a range of dates.
- Booking must check for date overlaps.
- Status: pending, confirmed, canceled.

### 🔹 API Endpoints

#### POST /api/bookings
- **Input**: `{ "property_id": "uuid", "start_date": "2025-07-01", "end_date": "2025-07-05" }`
- **Validation**:
  - Dates must be in the future.
  - End date must be after start date.
  - Check for availability
- **Output**: `{ "booking_id": "uuid", "status": "pending" }`

#### GET /api/bookings/:id
- **Output**: Booking details with dates and status

#### PATCH /api/bookings/:id/status
- **Input**: `{ "status": "confirmed" }`
- **Authorization**: Host only

### 🔹 Performance
- Booking conflict check must return < 500ms
- All booking queries filtered by indexed `property_id`

---

## Non-Functional Requirements
- All APIs must use HTTPS
- JSON format for all inputs and outputs
- Rate limiting applied on authentication endpoints
- Use PostgreSQL with foreign key constraints and indexing

