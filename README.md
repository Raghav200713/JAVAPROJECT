# DriveRent — Vehicle Rental Management Admin Dashboard

A commercial-grade, responsive **Vehicle Rental Management Admin Dashboard** built with **React**, **Vite**, **Tailwind CSS**, **Recharts**, **Lucide Icons**, and **Axios**.

Designed as a modern SaaS management platform inspired by clean purple-gradient dashboard aesthetics, this application manages the full vehicle rental lifecycle: fleet inventory, schedule coordination, collision-proof booking dispatch, customer driver license verification, GST invoice calculations, security deposit escrows, and vehicle maintenance locking.

> **Architecture Readiness**: The frontend features a centralized API service layer ready for seamless plug-and-play connection with a **Java Spring Boot REST API** backend with **MySQL** and **Spring Data JPA**.

---

## 🚀 Key Features

### 1. Modern SaaS Dashboard & Analytics
- **Purple Gradient Brand Visual Language**: Deep purple gradient sidebar (`#6b21a8` to `#3b0764`), crisp white cards with rounded corners (`rounded-2xl`), subtle shadows, and modern typography.
- **Dynamic KPI Cards**: Total Fleet Assets, Active On-Road Rentals, Available Vehicles, Pending Bookings, and Gross Revenue (calculated dynamically in INR ₹ Lakhs).
- **Fleet Availability Donut Chart**: Interactive Recharts donut visualization showing live status distribution:
  - 🟢 **Available** (Green)
  - 🟣 **Rented** (Purple)
  - 🔵 **Reserved** (Blue)
  - 🟠 **Under Maintenance** (Orange)
  - 🔴 **Out of Service** (Red)
- **Rental Overview Chart**: Progression across Confirmed, Picked Up, Active, Completed, and Cancelled stages.
- **Revenue Analytics**: Interactive Area Chart with instant period switching (**7 Days**, **30 Days**, **12 Months**).
- **Recent Bookings Stream**: Live data table showing recent reservations with instant status badges.

### 2. Fleet & Vehicle Management
- Complete vehicle parameters:
  - **Identification**: Vehicle ID, Make, Model, Year, Color, Type (*Sedan, SUV, Hatchback, Van, Luxury, Electric, Motorcycle*).
  - **Statutory / Legal**: License Plate, VIN (Chassis), Insurance Expiry Date with advance warning flags.
  - **Tariffs**: Base Daily Rate (₹), Hourly Rate (₹), Refundable Security Deposit (₹).
  - **Condition**: Odometer Mileage (km), Fuel / Battery Level (%) indicator, Pre-existing Damage Notes.
  - **Operations**: Hub Assignment, Live Status.
- Multi-parameter filtering, searching, sorting, and pagination.
- **Add / Edit Vehicle Form**: Structured into 5 distinct parameter sections with validation (unique IDs, positive rates, valid year).
- **Vehicle Profile View**: Detailed asset profile with tabbed history for Bookings, Maintenance Logs, and Revenue Generated.

### 3. Collision-Proof Booking & Scheduling
- **Strict Real-Time Availability Checking**: Automatically checks requested dates against existing bookings and maintenance schedules to **strictly prevent double booking**.
- **Alternative Vehicle Recommendations**: If a chosen vehicle is booked during the requested window, the system flags the collision and suggests other available vehicles in the same vehicle category.
- **Maintenance Lock**: Vehicles under servicing or out of service cannot be selected for reservations.
- **Dynamic Tariff & Invoice Calculator**:
  - Automatically calculates rental days based on pick-up and drop-off timestamps.
  - Computes base cost, optional add-ons (*GPS Navigation, Child Seat, Additional Driver, Zero-Dep Insurance, Roadside Assistance*).
  - Automatically calculates **18% Statutory GST** and holds the security deposit.
- **Lifecycle Status Transitions**:
  - `Pending` ➔ `Confirmed` ➔ `Active (Picked Up)` ➔ `Completed (Returned)`
  - `Pending / Confirmed` ➔ `Cancelled` (releases vehicle immediately and refunds authorized deposits).

### 4. Customer Directory & ID Verification
- Complete profiles with driver license numbers, expiry dates, contact details, and residential city.
- **Verification Status**: `Verified` vs `Unverified` customer badges.
- **Risk Flagging System**: Flag accounts for *Late Return*, *Vehicle Damage*, *Payment Issues*, or *Documentation Problems*.
- Customer profile view displaying lifetime spend, rental history, and flag history.

### 5. Financial & Payment Module
- Itemized invoice ledger tracking Base Cost, 18% GST, Insurance, Add-ons, Late Fines, and Security Deposit.
- Payment Gateways ready for **Stripe / Razorpay / UPI / Cards** using sanitized token placeholders (zero raw card or CVV exposure in frontend).
- Security Deposit status tracking (`Authorized`, `Released`, `Refunded`).
- One-click **Export Transactions to CSV** and printable GST tax invoice.

### 6. Vehicle Maintenance & Servicing
- Service types: *Oil Change, Brake Service, Tire Replacement, Engine Service, General Inspection, Repair, Cleaning*.
- **Automatic Vehicle Locking**: Scheduling or starting a maintenance service automatically updates the vehicle status to `Under Maintenance` and locks it from being booked.
- **Automatic Release**: Marking maintenance as `Completed` restores the vehicle status to `Available`.

### 7. Branch & Station Hubs
- Realistic Indian metro stations:
  - **Chennai Central Hub** (Chennai, Tamil Nadu)
  - **Bangalore Indiranagar Station** (Bangalore, Karnataka)
  - **Hyderabad HITEC City Station** (Hyderabad, Telangana)
  - **Coimbatore Airport Station** (Coimbatore, Tamil Nadu)
  - **Pondicherry Beach Road Hub** (Puducherry)
- Live metrics per branch: stationed vehicles, currently available, active rentals.

### 8. Global Search & Notifications
- Top header search bar with **instant multi-model search** across Vehicles, Customers, Bookings, and Transactions with direct click-through.
- Notification bell with unread badge count, insurance expiry reminders, upcoming pickups, and refund alerts.
- Role switcher (`Admin`, `Fleet Manager`, `Staff`) with prototype reset option.

---

## 🛠️ Technology Stack

| Layer | Technologies |
|---|---|
| **Framework** | React 19 + Vite |
| **Styling** | Tailwind CSS 3.4 (Custom design tokens, purple gradient theme) |
| **Icons** | Lucide React |
| **Charts** | Recharts (ResponsiveContainer, BarChart, AreaChart, PieChart) |
| **Routing** | React Router DOM v7 |
| **Networking** | Axios (Centralized API client with interceptors) |
| **State** | React Context (`RentalProvider`) + LocalStorage Persistent Mock DB |

---

## 📂 Project Structure

```
vehicle-rental-system/
├── public/
├── src/
│   ├── api/
│   │   └── client.js              # Centralized Axios client for Spring Boot
│   ├── assets/
│   ├── components/
│   │   ├── common/
│   │   │   ├── ConfirmDialog.jsx   # Reusable action confirmation modal
│   │   │   ├── DataTable.jsx       # Sortable, paginated, responsive data table
│   │   │   ├── EmptyState.jsx      # Empty state graphics and action triggers
│   │   │   ├── LoadingSkeleton.jsx # Pulse skeleton loaders
│   │   │   ├── Modal.jsx           # Accessible backdrop-blurred modal dialog
│   │   │   ├── StatCard.jsx        # Rounded KPI card with trends and icons
│   │   │   └── StatusBadge.jsx     # Status badge with semantic color dots
│   ├── context/
│   │   └── RentalContext.jsx       # Global application state and notifications
│   ├── data/
│   │   └── mockData.js             # Realistic sample dataset (Vehicles, Customers, etc.)
│   ├── layouts/
│   │   ├── DashboardLayout.jsx     # Persistent layout wrapper
│   │   ├── Header.jsx              # Global search, notifications, quick actions
│   │   └── Sidebar.jsx             # Purple gradient navigation sidebar
│   ├── pages/
│   │   ├── DashboardPage.jsx       # Main dashboard with KPI cards and charts
│   │   ├── VehiclesPage.jsx        # Fleet data table and filters
│   │   ├── VehicleDetailPage.jsx   # Vehicle profile with tabbed history
│   │   ├── VehicleFormPage.jsx     # 5-section Add/Edit vehicle form
│   │   ├── BookingsPage.jsx        # Bookings ledger with lifecycle actions
│   │   ├── BookingCreatePage.jsx   # New booking form with collision prevention
│   │   ├── BookingDetailPage.jsx   # Reservation breakdown and status transitions
│   │   ├── CustomersPage.jsx       # Customer directory with license verification
│   │   ├── CustomerDetailPage.jsx  # Customer profile, flags, and bookings
│   │   ├── PaymentsPage.jsx        # Financial ledger and transaction stats
│   │   ├── PaymentDetailPage.jsx   # Itemized invoice & deposit release
│   │   ├── MaintenancePage.jsx     # Workshop service dispatch & vehicle locking
│   │   ├── BranchesPage.jsx        # Multi-location hub stations
│   │   ├── ReportsPage.jsx         # Executive analytics, reports, CSV export
│   │   ├── SettingsPage.jsx        # System settings, GST tax, operational rules
│   │   ├── LoginPage.jsx           # Modern authentication page with demo roles
│   │   └── NotFoundPage.jsx        # 404 page
│   ├── services/
│   │   ├── apiService.js           # REST API functions with dual Spring Boot & mock mode
│   │   └── storageService.js       # Persistent browser database adapter
│   ├── App.jsx                     # Route definitions
│   ├── index.css                   # Tailwind directives and custom scrollbars
│   └── main.jsx                    # Application entry point
├── .env                            # Environment variables (API Base URL)
├── .env.example                    # Example configuration for Java backend
├── tailwind.config.js              # Theme color extension & font tokens
├── postcss.config.js
└── package.json
```

---

## ⚡ Installation & Running Instructions

### Prerequisites
- Node.js (v18+ recommended, v20+ supported)
- npm or yarn

### 1. Navigate to the project directory
```bash
cd "C:\Users\rghvs\.gemini\antigravity\scratch\vehicle-rental-system"
```

### 2. Install dependencies (if not already installed)
```bash
npm install
```

### 3. Run the development server
```bash
npm run dev
```

Open your browser and navigate to the displayed local URL (typically `http://localhost:5173`).

---

## ☕ Connecting to a Java Spring Boot REST Backend

The frontend is architected specifically to connect to a **Java Spring Boot 3.x** backend.

### 1. Switch from Mock to Backend Mode
Open `.env` (or copy `.env.example` to `.env`):
```env
VITE_API_BASE_URL=http://localhost:8080/api
VITE_USE_MOCK=false
```

### 2. Spring Boot Controller Endpoints Matrix
The `src/services/apiService.js` layer aligns with the following standard Spring Boot REST controller mappings:

| Endpoint | HTTP Method | Description |
|---|---|---|
| `/api/vehicles` | `GET` | List vehicles with query filters (`status`, `type`, `search`) |
| `/api/vehicles` | `POST` | Register a new fleet vehicle |
| `/api/vehicles/{id}` | `GET` | Retrieve vehicle by ID |
| `/api/vehicles/{id}` | `PUT` | Update vehicle parameters |
| `/api/vehicles/{id}` | `DELETE` | Delete vehicle (validated against active bookings) |
| `/api/bookings` | `GET` | Retrieve bookings with status filter |
| `/api/bookings` | `POST` | Create booking (checks collision and locks vehicle) |
| `/api/bookings/{id}` | `GET` | Retrieve booking details |
| `/api/bookings/{id}/status` | `PUT` | Transition booking status (`Active`, `Completed`, `Cancelled`) |
| `/api/customers` | `GET` | List customers with verification filters |
| `/api/customers` | `POST` | Register customer |
| `/api/customers/{id}` | `GET` | Get customer profile & booking history |
| `/api/customers/{id}` | `PUT` | Update customer record |
| `/api/payments` | `GET` | Retrieve financial transactions |
| `/api/payments/{id}` | `GET` | Retrieve payment breakdown and invoice |
| `/api/maintenance` | `GET` | Retrieve vehicle maintenance logs |
| `/api/maintenance` | `POST` | Schedule service (locks vehicle to Under Maintenance) |
| `/api/branches` | `GET` | Retrieve operational hub stations |
| `/api/dashboard/statistics` | `GET` | Retrieve aggregated operational metrics & chart points |

### 3. Spring Boot CORS Configuration
Ensure your Spring Boot application includes a `@CrossOrigin` or `WebMvcConfigurer` bean:
```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/api/**")
                .allowedOrigins("http://localhost:5173")
                .allowedMethods("GET", "POST", "PUT", "DELETE", "OPTIONS")
                .allowedHeaders("*")
                .allowCredentials(true);
    }
}
```

---

## 🛡️ Prototype Reset
At any time, you can reset all mock vehicles, customers, bookings, and payments back to the initial seed state by clicking the user avatar in the top right header and selecting **"Reset Prototype Data"**, or through **Settings ➔ Operational Policies ➔ Reset Local Mock Database**.
