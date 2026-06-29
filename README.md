# TransitGo — Bus & Train Ticket Booking System

A fully functional, single-file web application for booking bus and train tickets online. Built from the Software Requirements Specification (SRS) document.

---

## 📁 Project Structure

```
ticket-booking.html   ← Complete single-file web app (HTML + CSS + JS)
README.md             ← This file
```

---

## 🚀 Getting Started

### Run Locally

No installation required. Just open the file in any modern browser:

```bash
# Option 1 — Double-click the file
ticket-booking.html

# Option 2 — Serve with Python (recommended)
python3 -m http.server 8080
# Then open: http://localhost:8080/ticket-booking.html

# Option 3 — Serve with Node.js
npx serve .
```

### Deploy Online

Upload `ticket-booking.html` to any static hosting service:

| Platform | Steps |
|---|---|
| **GitHub Pages** | Push to a repo → Settings → Pages → Deploy from branch |
| **Netlify** | Drag and drop the file at netlify.com/drop |
| **Vercel** | `npx vercel --prod` in the project folder |
| **Cloudflare Pages** | Connect repo or upload directly |

---

## ✨ Features

### User Features
| Feature | Status |
|---|---|
| User Registration & Login | ✅ |
| Search Buses & Trains | ✅ |
| Filter by Morning / Evening / AC / Sleeper / Lowest Fare | ✅ |
| Interactive Seat Map | ✅ |
| 4-Step Booking Flow (Seat → Passenger → Payment → Confirm) | ✅ |
| Multiple Payment Methods (UPI, Card, Net Banking, Wallet) | ✅ |
| E-Ticket Generation with QR Code | ✅ |
| Booking History | ✅ |
| Booking Cancellation | ✅ |
| PDF Ticket Download (UI demo) | ✅ |

### Design Features
- Split-flap destination board animation (hero)
- Responsive layout — works on mobile, tablet, and desktop
- Slide-up modal panels for booking and auth flows
- Toast notifications for all user actions
- Gradient route line visualization on result cards

---

## 🗂️ Functional Requirements Coverage

Based on the SRS document:

| FR | Requirement | Implemented |
|---|---|---|
| FR1 | User Registration | ✅ |
| FR2 | User Login / Forgot Password | ✅ |
| FR3 | Search Tickets (source, destination, date, type) | ✅ |
| FR4 | Seat Selection with availability display | ✅ |
| FR5 | Booking with passenger details and booking ID | ✅ |
| FR6 | Payment — UPI, Card, Net Banking, Wallet | ✅ |
| FR7 | E-Ticket with QR code and booking details | ✅ |
| FR8 | Booking History with download | ✅ |
| FR9 | Cancellation with refund messaging | ✅ |
| FR10–FR16 | Admin dashboard, reports, route/bus/train management | 🔲 Backend required |

> FR10–FR16 (Admin features) require a backend server and database. See the Backend Integration section below.

---

## 🛠️ Technology Stack (Current — Frontend Only)

| Layer | Technology |
|---|---|
| Markup | HTML5 |
| Styling | CSS3 (custom properties, grid, flexbox, animations) |
| Logic | Vanilla JavaScript (ES6+) |
| Fonts | Space Grotesk, Inter, Space Mono (Google Fonts) |
| State | In-memory JavaScript objects |

---

## 🏗️ Upgrade to Full-Stack (Recommended Stack from SRS)

To add a real database, authentication, and admin panel, integrate the following:

### Backend — Node.js + Express

```bash
npm init -y
npm install express mysql2 jsonwebtoken bcrypt nodemailer cors dotenv
```

```
/backend
  /routes
    auth.js         ← Register, Login, OTP
    tickets.js      ← Search, Book, Cancel
    admin.js        ← Manage buses, trains, routes
  /models
    User.js
    Booking.js
    Bus.js
    Train.js
  /middleware
    auth.js         ← JWT verification
  server.js
```

### Database — MySQL Schema (from SRS)

```sql
CREATE TABLE users (
  user_id     INT AUTO_INCREMENT PRIMARY KEY,
  name        VARCHAR(100),
  email       VARCHAR(100) UNIQUE,
  mobile      VARCHAR(15),
  password    VARCHAR(255)   -- bcrypt hashed
);

CREATE TABLE buses (
  bus_id      INT AUTO_INCREMENT PRIMARY KEY,
  bus_name    VARCHAR(100),
  route       VARCHAR(200),
  fare        DECIMAL(10,2),
  seats       INT
);

CREATE TABLE trains (
  train_id    INT AUTO_INCREMENT PRIMARY KEY,
  train_name  VARCHAR(100),
  route       VARCHAR(200),
  coach       VARCHAR(50),
  fare        DECIMAL(10,2)
);

CREATE TABLE bookings (
  booking_id      INT AUTO_INCREMENT PRIMARY KEY,
  user_id         INT,
  vehicle_id      INT,
  passenger_name  VARCHAR(100),
  seat_number     VARCHAR(10),
  journey_date    DATE,
  payment_status  ENUM('pending','paid','refunded')
);

CREATE TABLE payments (
  payment_id      INT AUTO_INCREMENT PRIMARY KEY,
  booking_id      INT,
  amount          DECIMAL(10,2),
  payment_method  VARCHAR(50),
  status          ENUM('pending','success','failed')
);
```

### Replace In-Memory State with API Calls

```javascript
// Example: Search tickets
async function doSearch() {
  const res = await fetch(`/api/tickets/search?from=${from}&to=${to}&date=${date}&mode=${mode}`);
  const data = await res.json();
  renderResults(data.services);
}

// Example: Create booking
async function procBooking() {
  const res = await fetch('/api/bookings', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json', 'Authorization': `Bearer ${token}` },
    body: JSON.stringify({ serviceId, seat, passenger, paymentMethod })
  });
  const booking = await res.json();
  renderTicket(booking);
}
```

---

## 🔮 Future Enhancements (from SRS)

- [ ] Live bus / train tracking (GPS integration)
- [ ] AI chatbot for booking assistance
- [ ] Multi-language support (i18n)
- [ ] Wallet system with balance
- [ ] Loyalty rewards / points
- [ ] Push notifications (Web Push API)
- [ ] Mobile app (React Native or Flutter)
- [ ] Facial recognition login
- [ ] AI-based ticket recommendations

---

## 🔐 Security Notes (for Production)

- Store passwords with **bcrypt** (never plain text)
- Use **JWT** tokens with expiry for sessions
- Enforce **HTTPS** with SSL certificates
- Implement **rate limiting** on auth and search endpoints
- Use **parameterized SQL queries** to prevent injection
- Enable **CORS** only for trusted origins
- Add **input validation** on all form fields

---

## 📱 Browser Compatibility

| Browser | Supported |
|---|---|
| Chrome 90+ | ✅ |
| Firefox 88+ | ✅ |
| Edge 90+ | ✅ |
| Safari 14+ | ✅ |
| Mobile Chrome / Safari | ✅ |

---

## 📄 License

This project is provided for educational and development purposes based on the accompanying SRS document.

---

## 👤 Author

Built with TransitGo Design System  
Based on IEEE-style SRS for Bus & Train Ticket Booking System
