# Cino – Cinema Booking Platform

A full-stack cinema booking project built with Node.js, Express.js, MongoDB, and Vue.js. The backend syncs Apollo Kino data, and the frontend consumes the REST API for browsing films, schedules, and bookings.

## Table of Contents
- [Project Overview](#project-overview)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Environment Variables](#environment-variables)
- [Running the Project](#running-the-project)
- [Frontend](#frontend)
- [Backend](#backend)
- [Database Schema](#database-schema)
- [Project Structure](#project-structure)
- [Available Scripts](#available-scripts)
- [API Endpoints](#api-endpoints)
- [Technologies Used](#technologies-used)

## Project Overview

- Public catalogue of films, sessions, news, and schedules
- Booking flow with seat selection and booking number lookup
- Admin dashboard for managing sessions and bookings (no authentication yet)
- Apollo Kino XML API integration for data sync

## Prerequisites

Before you begin, ensure you have the following installed on your system:

- **Node.js** (v14 or higher recommended, tested with v20.19.5)
- **npm** (v6 or higher, comes with Node.js)
- **MongoDB** (v4.0 or higher) - Required for database operations

To check if you have Node.js and npm installed, run:

```bash
node --version
npm --version
```

To check if MongoDB is running:

```bash
mongosh --eval "db.version()"
```

## Installation

### Backend Installation

1. Clone the repository:
```bash
git clone https://github.com/DLtru/cino.git
cd cino
```

2. Install backend dependencies:
```bash
npm install
```

This will install all required packages listed in `package.json`.

### Frontend Installation

3. Install frontend dependencies:
```bash
cd frontend
npm install
```

This will install Vue 3, Vue Router, and Vite.

## Environment Variables

Create a `.env` file in the repository root for the backend:

```bash
MONGODB_URI=mongodb://localhost:27017/cinema
PORT=3000
APOLLO_KINO_API_URL=https://www.apollokino.ee/xml
```

- `MONGODB_URI` is required. The server exits if it is not set.
- `PORT` and `APOLLO_KINO_API_URL` are optional.

For the frontend, create `frontend/.env` (or `.env.local`) if you need a custom API base URL:

```bash
VITE_API_URL=http://localhost:3000
```

## Running the Project

### Backend

#### Production Mode

To start the backend server in production mode:

```bash
npm start
```

The server will start on `http://localhost:3000`

#### Development Mode

To start the backend server in development mode with auto-restart on file changes:

```bash
npm run dev
```

This uses `nodemon` to automatically restart the server when you make changes to the code.

### Frontend

#### Development Mode

To start the frontend development server:

```bash
cd frontend
npm run dev
```

The application will be available at `http://localhost:5173`

#### Production Build

To build the frontend for production:

```bash
cd frontend
npm run build
```

The built files will be in the `frontend/dist` directory.

## Frontend

The frontend is a Vue 3 application with the following views:

- **Home Page** (`/`) - Hero section with featured content
- **Movies Page** (`/movies`) - Browse all movies with filtering options
- **Movie Detail Page** (`/movies/:id`) - Detailed movie information with showtimes
- **Schedule Page** (`/schedule`) - Daily schedule overview
- **News Page** (`/news`) - Apollo Kino news feed
- **Booking Page** (`/booking`) - Multi-step booking process
- **Admin Dashboard** (`/admin`) - Manage movies, sessions, bookings, and cinemas

The frontend reads `VITE_API_URL` to connect to the backend (defaults to `http://localhost:3000`).

For more details, see [frontend/README.md](frontend/README.md).

## Backend

The backend is an Express.js API that connects to MongoDB, syncs data from the Apollo Kino XML API, and serves booking and admin workflows. Admin endpoints are currently unauthenticated, so treat them as trusted/internal routes.

## Database Schema

This project includes a comprehensive database schema for a cinema booking system. The schema includes 7 main models:

- **User** - User authentication and profile management
- **Cinema** - Physical cinema locations
- **Hall** - Screening rooms within cinemas
- **Film** - Movie information and metadata
- **Session** - Movie showtimes and pricing
- **Seat** - Individual seats in halls
- **Booking** - Ticket reservations and payments

For detailed information about the database schema, including field descriptions, relationships, and usage examples, see [DATABASE_SCHEMA.md](DATABASE_SCHEMA.md).

## Project Structure

```
cino/
├── frontend/               # Vue.js frontend application
│   ├── src/
│   │   ├── components/    # Reusable Vue components
│   │   │   └── layout/    # Layout components (Header, Footer)
│   │   ├── views/         # Page components
│   │   │   ├── Booking.vue
│   │   │   ├── Home.vue
│   │   │   ├── Movies.vue
│   │   │   ├── MovieDetail.vue
│   │   │   ├── News.vue
│   │   │   ├── Schedule.vue
│   │   │   └── Admin.vue
│   │   ├── router/        # Vue Router configuration
│   │   ├── App.vue        # Root component
│   │   └── main.js        # Application entry point
│   ├── public/            # Static assets
│   ├── index.html         # HTML template
│   ├── vite.config.js     # Vite configuration
│   └── package.json       # Frontend dependencies
├── backend/
│   ├── Models/            # Database models (Mongoose schemas)
│   │   ├── User.js        # User model
│   │   ├── Cinema.js      # Cinema model
│   │   ├── Hall.js        # Hall model
│   │   ├── Film.js        # Film model
│   │   ├── Session.js     # Session model
│   │   ├── Seat.js        # Seat model
│   │   ├── Booking.js     # Booking model
│   │   └── index.js       # Models export file
│   ├── services/          # External API integrations
│   └── index.js           # Main server file with Express configuration
├── node_modules/          # Backend dependencies (not tracked in git)
├── .gitignore             # Git ignore rules
├── DATABASE_SCHEMA.md     # Database schema documentation
├── docs/                  # Additional documentation
├── package.json           # Backend metadata and dependencies
├── package-lock.json  # Locked versions of dependencies
└── README.md          # This file
```

## Available Scripts

### Backend Scripts

- `npm start` - Starts the backend server in production mode
- `npm run dev` - Starts the backend server in development mode with nodemon
- `npm run seed` - Seed the database with initial data
- `npm run add-sessions` - Add sessions to the database
- `npm test` - Placeholder for tests (not yet implemented)

### Frontend Scripts

- `cd frontend && npm run dev` - Starts the frontend development server
- `cd frontend && npm run build` - Builds the frontend for production
- `cd frontend && npm run preview` - Preview the production build

## API Endpoints

Base URL: `http://localhost:3000`

### Core

- `GET /` - Health check (returns `"abbik"`)

### Films

- `GET /api/films` (filters: `genre`, `ageRating`, `limit`)
- `GET /api/films/:id`
- `GET /api/films/:id/sessions`

### Sessions

- `GET /api/sessions` (filters: `filmId`, `date`, `hallId`)
- `GET /api/sessions/:id/seats`

### Cinemas

Rate limited to 60 requests per minute.

- `GET /api/cinemas`
- `GET /api/cinemas/:id/halls`

### Apollo Kino Integration

- `GET /api/apollo-kino/sync` (query: `dtFrom`, `dtTo`)
- `GET /api/apollo-kino/raw`
- `GET /api/apollo-kino/events`
- `GET /api/apollo-kino/schedule` (query: `dtFrom`, `dtTo`)
- `GET /api/apollo-kino/TheatreAreas`
- `GET /api/apollo-kino/NewsCategories`
- `GET /api/apollo-kino/News`

### Bookings

- `POST /api/bookings` (body: `sessionId`, `seats`, `contactEmail`, `contactPhone`, `paymentMethod`)
- `GET /api/bookings/:bookingNumber`

### Admin (no authentication yet)

- `GET /api/admin/sessions`
- `GET /api/admin/sessions/:id`
- `POST /api/admin/sessions`
- `PUT /api/admin/sessions/:id`
- `DELETE /api/admin/sessions/:id`
- `GET /api/admin/halls`
- `GET /api/admin/bookings`
- `DELETE /api/admin/bookings/:id`

## Technologies Used

### Frontend

- **Vue 3** (v3.5.22) - Progressive JavaScript framework
- **Vue Router 4** (v4.6.3) - Official router for Vue.js
- **Vite** (v7.1.7) - Next generation frontend tooling

### Backend

- **Node.js** - JavaScript runtime
- **Express.js** (v5.1.0) - Web application framework
- **CORS** (v2.8.5) - Middleware for enabling CORS
- **Mongoose** (v8.19.2) - MongoDB ODM
- **Nodemon** (v3.1.10) - Development tool for auto-restarting

## License

ISC

## Contributing

This is a school project for Tallinna Polütehnikum. For issues and questions, please use the [GitHub Issues](https://github.com/DLtru/cino/issues) page.
