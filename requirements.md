# 📌 Airbnb Clone Backend – Feature Requirements

This document specifies the technical and functional requirements for three core backend features: User Authentication, Property Management, and the Booking System.

## 1. 🔐 User Authentication

### Functional Requirements:
- Allow users to register as guest or host.
- Authenticate using email/password or OAuth (Google, Facebook).
- Maintain session using JWT.
- Allow users to update their profile and reset passwords.

### API Endpoints:

| Method | Endpoint           | Description                  |
|--------|--------------------|------------------------------|
| POST   | `/api/register`    | Register a new user          |
| POST   | `/api/login`       | Log in with email/password   |
| POST   | `/api/oauth/login` | Log in via Google/Facebook   |
| GET    | `/api/profile`     | Fetch current user profile   |
| PUT    | `/api/profile`     | Update profile info          |
| POST   | `/api/logout`      | Log out user (invalidate JWT)|

### Input/Output Specs:
- `email`: string, valid email format.
- `password`: string, min 8 characters.
- `role`: enum ['guest', 'host'].

### Validation Rules:
- Unique email per user.
- Passwords must be hashed (bcrypt).

### Performance Criteria:
- Response time < 300ms.
- Max 5 login attempts/minute (rate limiting).

## 2. 🏠 Property Management

### Functional Requirements:
- Hosts can list properties with details.
- Listings include title, location, price, images, and amenities.
- Hosts can edit or delete their listings.

### API Endpoints:

| Method | Endpoint               | Description                    |
|--------|------------------------|--------------------------------|
| POST   | `/api/properties`      | Create a new listing           |
| GET    | `/api/properties`      | Retrieve all listings          |
| GET    | `/api/properties/:id`  | Retrieve a specific listing    |
| PUT    | `/api/properties/:id`  | Update listing details         |
| DELETE | `/api/properties/:id`  | Delete a listing               |

### Input/Output Specs:
- `title`: string, required.
- `price`: float, must be > 0.
- `location`: string, required.
- `amenities`: array of strings.

### Validation Rules:
- Title must be unique per host.
- Maximum 20 images per listing.
- Price must be validated for numeric input.

### Performance Criteria:
- Full-text search enabled on `location`, `title`.
- Response time < 500ms for read operations.

## 3. 📅 Booking System

### Functional Requirements:
- Guests can book available properties for specific dates.
- System prevents overlapping bookings.
- Booking status: pending, confirmed, canceled, completed.

### API Endpoints:

| Method | Endpoint                | Description                 |
|--------|-------------------------|-----------------------------|
| POST   | `/api/bookings`         | Create a new booking        |
| GET    | `/api/bookings`         | List user bookings          |
| GET    | `/api/bookings/:id`     | View booking details        |
| PATCH  | `/api/bookings/:id`     | Update status or cancel     |

### Input/Output Specs:
- `property_id`: UUID, required.
- `start_date`, `end_date`: ISO date format.
- `status`: enum ['pending', 'confirmed', 'canceled', 'completed'].

### Validation Rules:
- Prevent double bookings (check date overlap).
- Start date must be before end date.
- Properties cannot be booked for past dates.

### Performance Criteria:
- DB indexes on `property_id`, `start_date`.
- Conflict checks must complete in < 200ms.

✅ **Note**: All endpoints must return appropriate HTTP status codes (e.g., 200, 201, 400, 401, 404, 409, 500).

