# Requirement specifications for three key backend features of the Airbnb Clone project: 

## 🔐 1. User Authentication

### 📌 Feature: User Registration & Login

**Endpoints:**

* `POST /api/auth/register`
* `POST /api/auth/login`

**Input:**

```json
POST /api/auth/register
{
  "email": "user@example.com",
  "password": "SecurePass123!",
  "role": "guest" // or "host"
}
```

```json
POST /api/auth/login
{
  "email": "user@example.com",
  "password": "SecurePass123!"
}
```

**Output:**

* **200 OK** (Login success):

```json
{
  "token": "jwt_token",
  "user": {
    "id": 1,
    "email": "user@example.com",
    "role": "guest"
  }
}
```

* **201 Created** (Registration success):

```json
{
  "message": "User registered successfully"
}
```

**Validation Rules:**

* Valid email format
* Password must be at least 8 characters, with a number and special character
* Role must be either "guest" or "host"

**Performance Criteria:**

* Token generation should occur in under 200ms
* Rate limit login attempts to prevent brute-force attacks (e.g., 5 per minute)

---

## 🏠 2. Property Management

### 📌 Feature: Create & Manage Listings

**Endpoints:**

* `POST /api/properties/`
* `PUT /api/properties/:id/`
* `DELETE /api/properties/:id/`
* `GET /api/properties/?location=Paris&guests=2`

**Input:**

```json
POST /api/properties/
{
  "title": "Modern Apartment in Paris",
  "description": "Cozy and well-located",
  "location": "Paris",
  "price_per_night": 120,
  "amenities": ["wifi", "kitchen"],
  "max_guests": 3,
  "available_dates": ["2025-06-01", "2025-06-15"]
}
```

**Output:**

```json
{
  "id": 10,
  "title": "Modern Apartment in Paris",
  "host_id": 5,
  "created_at": "2025-05-12T10:30:00Z"
}
```

**Validation Rules:**

* Price must be a positive number
* `max_guests` must be at least 1
* `title`, `location`, `description` must not be empty

**Performance Criteria:**

* Listings fetch should support pagination (e.g., 20 per page)
* Response time < 300ms for search with filters

---

## 📅 3. Booking System

### 📌 Feature: Create & Manage Bookings

**Endpoints:**

* `POST /api/bookings/`
* `GET /api/bookings/:id/`
* `DELETE /api/bookings/:id/`

**Input:**

```json
POST /api/bookings/
{
  "property_id": 10,
  "check_in": "2025-06-05",
  "check_out": "2025-06-10",
  "guest_id": 2
}
```

**Output:**

```json
{
  "booking_id": 25,
  "status": "confirmed",
  "total_cost": 600
}
```

**Validation Rules:**

* `check_in` must be before `check_out`
* Property must be available for selected dates
* Booking must not overlap with existing bookings

**Performance Criteria:**

* Ensure booking creation under 500ms with conflict check
* Prevent race conditions using transactional integrity or row-level locking

