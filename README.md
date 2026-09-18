# BARBERO - Real-Time Neighborhood Barber Booking

`BARBERO` is a real-time, neighborhood-based barber booking service, inspired by inDrive, designed for seamless home service requests. It prioritizes speed, simplicity, and a high-end, minimalist experience.

---

## Features

*   **Ultra-Minimalist Design:** Dark Mode, clean geometric shapes, and a soft sage green (#8DA399) accent for calmness.
*   **Neighborhood Focus:** No complex maps. Users select their neighborhood, and requests are instantly routed to all barbers in that specific zone.
*   **Real-Time Broadcast (inDrive Style):** Utilizing WebSockets (Socket.io), new orders are pushed instantly to all active barbers in the relevant neighborhood "room."
*   **Protected Access:** Comprehensive authentication using JWT (JSON Web Tokens) and secure password hashing with bcrypt.

---

## Architecture Overview

The project is structured with a distinct separation of concerns, divided into two main parts:

### 1. Backend (Server)
Located in `/backend`. This is the core logic and data layer.

**Tech Stack:**
*   **Node.js & Express:** For the application and API.
*   **PostgreSQL:** Relational database for persistent storage.
*   **Sequelize (ORM):** To manage database models, relations, and migrations.
*   **Socket.io:** For real-time, bidirectional communication.
*   **JWT & bcrypt:** For security and authentication.

**Key Components:**
*   **ROUTER:** Receives requests and routes them (`/api/auth`, `/api/orders`, `/api/neighborhoods`).
*   **MIDDLEWARE:** Intercepts requests for authentication (verifies JWT) and validation (using schema).
*   **CONTROLLER:** The main business logic ("The Brain"). Handles data processing and communicates with the database and WebSockets.
*   **DATABASE:** The persistent data layer.
*   **WEBSOCKETS (SOCKET.IO):** Manages connection rooms (`neighborhood_${id}`). Emits new orders instantly to the correct room.

### 2. Frontend (Mobile App)
Located in `/mobile`. A React Native application built with Expo and TypeScript.

**Tech Stack:**
*   **React Native & Expo:** For cross-platform mobile development.
*   **TypeScript:** For typed, clean code.
*   **Socket.io-client:** To connect and listen for real-time events.

**Key Components:**
*   **CLIENT/UI:** Minimalist interface, focused on essential tasks (Login/Register, Create Order, View Order, Accept Order).
*   **AUTHENTICATION:** Protected routes, login/register forms, manages JWT.

---

## Database Schema (3 Key Tables)

**1. Neighborhoods**
*   `id` (PK)
*   `name` (e.g., "Hay Elmouahidine")

**2. Users**
*   `id` (PK)
*   `phone` (Unique)
*   `password` (Hashed)
*   `role` (ENUM: CUSTOMER, BARBER)
*   `neighborhood_id` (FK, Nullable - *for Barbers only*)

**3. Orders**
*   `id` (PK)
*   `customer_id` (FK)
*   `barber_id` (FK, Nullable - *set when accepted*)
*   `neighborhood_id` (FK)
*   `service_details` (JSON/TEXT)
*   `status` (ENUM: PENDING, ACCEPTED, COMPLETED, CANCELED)

---

## Setup and Usage

### Backend
1.  **Prerequisites:** Install Node.js, NPM, and PostgreSQL. Create an empty PostgreSQL database.
2.  **Clone the project:** `git clone ...` and `cd backend`.
3.  **Install dependencies:** `npm install`.
4.  **Configure environment:** Create a `.env` file based on `.env.example`. Set your database URL (`DB_URL`) and JWT secret (`JWT_SECRET`).
5.  **Run Migrations:** `npx sequelize-cli db:migrate` and `npx sequelize-cli db:seed:all` (optional, to seed neighborhoods).
6.  **Run the Server:** `npm start` (or `npm run dev` with nodemon).

### Mobile (Expo)
1.  **Prerequisites:** Install Node.js, NPM, and the Expo Go app on your phone.
2.  **Clone and cd:** `cd mobile`.
3.  **Install dependencies:** `npm install`.
4.  **Configure:** Update the API base URL in your config file to point to your backend server (e.g., `http://192.168.1.10:3000`).
5.  **Run the app:** `npx expo start`. Scan the QR code with your phone.

---

## Design Guidelines

*   **Mode:** Dark Mode
*   **Color Palette:**
    *   `Background`: `#121212` (Deep black/dark grey)
    *   `Primary Accent`: `#8DA399` (Soft Sage Green - used for buttons, icons, interactive elements to convey calmness)
    *   `Typography`: `#FFFFFF` (High-contrast white for headers/active text), `#A0A0A0` (Secondary grey for details)

---

## Contributing

Contributions are welcome. Please open an issue or submit a pull request for any bugs or features.

---

## License

This project is licensed under the MIT License.

---

## Support

For any questions or to ask for help, please contact the project author. We aim to support a complete, end-to-end guide.