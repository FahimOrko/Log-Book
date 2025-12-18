## Authorization Testing Report

### Application Information

- Target: Resource Reservation System
- URL: http://localhost:8003
- Testing type: Role-based authorization verification
- Roles tested: Guest, Reserver, Administrator

- Tools used:
  - Browser (manual testing)
  - OWASP ZAP
  - Endpoint guessing (manual URL manipulation)

---

## 🧑‍🦲 Guest (Not Logged In)

#### ✅ Can Do

- View public resource list — /

  - Observation: Resources and bookings are visible without login
  - Spec match: ✅ Yes (spec 8)

- View booked time slots without identity disclosure — /

  - Observation: Booked resources are shown without logging in
  - Spec match: ✅ yes (spec 8, GDPR)

- Access login page — /login

  - Observation: Login form accessible
  - Spec match: ✅ Yes

- Access registration page — /register

  - Observation: Registration form accessible
  - Spec match: ✅ Yes

#### Extra

- View endpoint resource — /resources

  - Observation: Resources page is visible without login but nothing happens when new resorces are added

#### ❌ Cannot Do

- Access reservation page — /reservation

  - Observation: Redirected to /login
  - Spec match: ✅ Yes

- Create reservations via API — /api/reservations (POST)

  - Observation: Request rejected when not authenticated
  - Spec match: ✅ Yes

- Access user profile — /profile

  - Observation: Redirected to login
  - Spec match: ✅ Yes

- Access admin pages — /admin/\*

  - Observation: Access denied / redirect
  - Spec match: ✅ Yes

---

## 🧑‍💼 Reserver (Authenticated User)

#### ✅ Can Do

- Log in to the system — /login

  - Observation: Successful authentication
  - Spec match: ✅ Yes

- View resource list — /resources

  - Observation: Resources form visible
  - Spec match: ✅ Yes

- Book a resource — /reservation, /api/reservations

  - Observation: Reservation can be created and /api/reservations retureens a json
  - Spec match: ✅ Yes (spec 6, 7)

- View own profile — /profile

  - Observation: Own user data not visible
  - Spec match: ❌ No (GDPR compliant)

#### ❌ Cannot Do

- Access admin dashboard — /admin

  - Observation: Access denied / redirect
  - Spec match: ✅ Yes

- Manage resources — /admin/resources

  - Observation: Access denied
  - Spec match: ✅ Yes (spec 4)

- Delete users — /api/admin/users/:id

  - Observation: Returns an error json object
  - Spec match: ✅ Yes (spec 5)

- View or modify other users’ reservations — /api/reservations/:id

  - Observation: Cannot edit only sends the resvervation info back
  - Spec match: ✅ Yes (authorization enforced)

---

## 🧑‍💼🛡️ Administrator

#### ✅ Can Do

- Access admin dashboard — /admin

  - Observation: Admin cannot access any dashboard while logged in
  - Spec match: ❌ No

- Create, modify, delete resources — /admin/resources/new

  - Observation: Actions succeed
  - Spec match: ✅ Yes (spec 4)

- View and manage all reservations — /admin/reservations

  - Observation: All reservations visible and editable
  - Spec match: ✅ Yes

- Delete reserver accounts — /admin/users/delete/:id

  - Observation: User deletion successful
  - Spec match: ✅ Yes (spec 5)

#### ❌ Cannot Do

- View plaintext passwords or sensitive credentials

  - Observation: No sensitive data exposed
  - Spec match: ✅ Yes (GDPR, PbD)
