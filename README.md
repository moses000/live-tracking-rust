Live Tracking Application
A modern, scalable live tracking application built with Rust for the backend and React with Mapbox for the frontend. This application supports real-time location updates, geofencing, proximity alerts, history playback, and robust security features. It leverages a high-performance architecture to ensure low latency and scalability.
Table of Contents

Features
Architecture
Prerequisites
Installation
Usage
Environment Variables
File Structure
Contributing
License

Features

Real-Time Tracking: Stream location updates via WebSockets for live map visualization.
Geofencing: Define circular geofences and trigger notifications when users enter/exit.
Proximity Alerts: Detect and notify users within 1000m of each other.
History Playback: Replay historical location data with speed controls (0.5x, 1x, 2x, 4x) and path visualization.
Security:
JWT-based authentication for all endpoints.
Rate limiting (100 requests/minute per IP).
Input validation with validator crate.
CORS restricted to frontend origin.
Secure headers and parameterized SQL queries.


Frontend: Interactive React interface with Mapbox for map rendering and real-time updates.
Scalable Backend: Built with Rust, Axum, Redis, and PostgreSQL/TimescaleDB for high performance.

Architecture
The application follows a modern, layered architecture designed for scalability and performance, visualized below using a Mermaid flowchart:
```
graph TD
    A[Clients Web/Mobile] -->|WebSocket/REST JWT Auth| B[Rust Backend Axum]
    B --> C[Auth Middleware JWT Validation, Rate Limiting]
    B --> D[WebSocket Handler Real-Time Updates]
    B --> E[Location Streaming Tokio Broadcast Channels]
    B --> F[Business Logic Geofencing, Proximity, History]
    B --> G[Data Layer]
    G --> H[Redis Real-Time Positions, GEOSEARCH]
    G --> I[PostgreSQL/TimescaleDB Historical Data, Geofences]
    A --> J[Frontend React + Mapbox]
    J --> K[Live Map View Real-Time Markers, Notifications]
    J --> L[History Playback Path Visualization, Speed Controls]
```
Backend Components

Framework: Axum for async HTTP and WebSocket handling.
Runtime: Tokio for asynchronous task management.
Real-Time Storage: Redis for current positions and geospatial queries.
Historical Storage: PostgreSQL with TimescaleDB for time-series location data.
Geospatial Logic: geo crate for distance calculations and geofencing.
Authentication: JWT with jsonwebtoken crate, validated via middleware.
Logging: tracing for structured logs, suitable for production monitoring.
Error Handling: Custom AppError with thiserror for robust error management.

Frontend Components

Framework: React with Vite for fast development and build.
Map Rendering: Mapbox GL JS for interactive maps and path visualization.
Real-Time Updates: WebSocket connection for live location and notifications.
Authentication: JWT token decoding with jwt-decode for user ID extraction.
Styling: Tailwind CSS for responsive, modern UI.

Prerequisites

Rust: Stable toolchain (install via rustup).
Node.js: Version 18+ (install via Node.js).
Redis: Running locally or via Docker.
PostgreSQL/TimescaleDB: Running locally or via Docker.
Mapbox Account: For access token (sign up at mapbox.com).
Docker (optional): For containerized deployment.

Installation

Clone the Repository:
git clone https://github.com/your-username/live-tracking.git
cd live-tracking


Set Up Environment Variables:Create a .env file in the root directory:
JWT_SECRET=your-secure-secret-key-here

Replace your-secure-secret-key-here with a strong secret key.

Set Up Dependencies:

Redis: Start Redis locally or via Docker:docker run -p 6379:6379 -d redis


PostgreSQL/TimescaleDB: Start TimescaleDB locally or via Docker:docker run -d --name timescaledb -p 5432:5432 -e POSTGRES_PASSWORD=postgres timescale/timescaledb:latest-pg16


Initialize the database:psql -h localhost -U postgres -d tracking -f sql/init.sql

Add a sample geofence:INSERT INTO geofences (id, name, center_lat, center_lon, radius_meters)
VALUES ('fence1', 'Home', 40.7128, -74.0060, 500);




Install Backend Dependencies:
cargo build


Install Frontend Dependencies:
cd frontend
npm install

Replace 'YOUR_MAPBOX_ACCESS_TOKEN' in frontend/src/Map.jsx and frontend/src/HistoryPlayback.jsx with your Mapbox access token.


Usage

Start the Backend:
cargo run

The server runs on http://localhost:3000.

Start the Frontend:
cd frontend
npm start

The frontend runs on http://localhost:5173.

Interact with the Application:

Open http://localhost:5173 in your browser.
Log in with username: admin, password: password (replace with secure credentials in production).
Live Map: Send locations, view real-time markers, geofence triggers, and proximity alerts.
History Playback: Select a time range, play/pause with speed controls (0.5x, 1x, 2x, 4x), and view path visualization.



Environment Variables



Variable
Description
Default



JWT_SECRET
Secret key for JWT signing
Must be set in .env


File Structure
live-tracking/
├── Cargo.toml            # Rust dependencies
├── .env                  # Environment variables
├── src/                  # Backend source code
│   ├── main.rs           # Entry point
│   ├── auth.rs           # Authentication logic
│   ├── db.rs             # Database connections
│   ├── models.rs         # Data models
│   ├── routes.rs         # API routes
│   ├── ws.rs             # WebSocket handling
│   ├── geo.rs            # Geospatial calculations
│   ├── middleware.rs     # Authentication and rate limiting
│   └── error.rs          # Error handling
├── sql/
│   └── init.sql          # Database schema
└── frontend/             # Frontend source code
    ├── index.html        # HTML entry point
    ├── package.json      # Node dependencies
    └── src/
        ├── App.jsx       # Main React component
        ├── Map.jsx       # Live map view
        ├── HistoryPlayback.jsx # History playback view
        ├── index.js      # React entry point
        └── utils.js      # Utility functions (JWT decoding)

Contributing
Contributions are welcome! Please follow these steps:

Fork the repository.
Create a feature branch (git checkout -b feature/your-feature).
Commit changes (git commit -m 'Add your feature').
Push to the branch (git push origin feature/your-feature).
Open a pull request.

Ensure code follows Rust and JavaScript best practices, and include tests where applicable.
License
This project is licensed under the MIT License. See the LICENSE file for details.
