# Hostel Management System

A full-stack Hostel Management System built with **Angular, Node.js, Express.js, and MongoDB**. The application helps manage hostel rooms, student records, room pricing, food menus, user accounts, and inquiries through public and admin interfaces.

## Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Installation and Setup](#installation-and-setup)
- [Database Setup](#database-setup)
- [Running the Application](#running-the-application)
- [Admin Account Setup](#admin-account-setup)
- [Application Routes](#application-routes)
- [Troubleshooting](#troubleshooting)
- [Available Commands](#available-commands)

## Features

- User registration and sign-in
- Public dashboard and hostel details
- Admin dashboard
- Student registration, viewing, updating, and removal
- Hostel room structure management
- Room availability tracking
- Hostel pricing management
- Food menu management
- Inquiry management
- User management
- User profile management
- MongoDB database integration

## Tech Stack

| Technology | Purpose |
|---|---|
| Angular | Frontend |
| Node.js | Backend runtime |
| Express.js | REST API |
| MongoDB | Database |
| Mongoose | MongoDB object modeling |
| TypeScript | Frontend development |
| HTML and CSS | User interface |

## Project Structure

```text
HostelManagementSystem-main/
├── Data/
│   ├── prices.json
│   ├── boyssuperdeluxerooms.json
│   ├── boysdeluxerooms.json
│   ├── boysstandardrooms.json
│   ├── girlssuperdeluxerooms.json
│   ├── girlsdeluxerooms.json
│   └── girlsstandardrooms.json
├── hostelManagement/
│   ├── server/
│   │   ├── config/
│   │   └── ...
│   ├── src/
│   │   └── app/
│   │       ├── admin/
│   │       ├── auth/
│   │       ├── dashboard/
│   │       ├── hostel-detail/
│   │       ├── contact-us/
│   │       └── profile/
│   ├── package.json
│   ├── .env.sample
│   └── ...
└── README.md
```

> Run all npm commands from the `hostelManagement` directory, where the application's `package.json` is located.

## Prerequisites

Install the following software before running the application:

- [Node.js and npm](https://nodejs.org/)
- [MongoDB Community Server](https://www.mongodb.com/try/download/community)
- [MongoDB Compass](https://www.mongodb.com/products/tools/compass) (recommended for database management)
- [Git](https://git-scm.com/) (optional, for cloning the repository)

**Note:** MongoDB Compass is a graphical database client. MongoDB Community Server must also be installed and running for the backend to connect to the database.

## Installation and Setup

### 1. Clone the repository

If you are using Git:

```bash
git clone <your-repository-url>
cd HostelManagementSystem-main/hostelManagement
```

Alternatively, download the repository as a ZIP file and extract it.

### 2. Install dependencies

Open a terminal in the `hostelManagement` directory and run:

```bash
npm install
```

### 3. Configure environment variables

Create a `.env` file inside the `hostelManagement` directory. Use `.env.sample` as a reference if it contains the required variables.

Example local configuration:

```dotenv
NODE_ENV=development
PORT=4050
MONGODB_URI=mongodb://localhost:27017/hostel
MONGOOSE_DEBUG=true
JWT_SECRET=replace-this-with-a-long-random-secret
```

Update `JWT_SECRET` with a strong, private random value.

**Security:** Do not commit your `.env` file or expose real secrets, passwords, or credentials in your repository. Keep `.env.sample` free of private values.

## Database Setup

### 1. Start MongoDB

Start the MongoDB Community Server service.

For a local installation, MongoDB Compass can connect using:

```text
mongodb://localhost:27017
```

The application uses a database named `hostel`.

### 2. Import the sample data

The repository contains sample JSON files in the `Data` directory. In MongoDB Compass:

1. Connect to `mongodb://localhost:27017`.
2. Open or create the `hostel` database.
3. Create the collection corresponding to each JSON file below, if it does not already exist.
4. Select the collection and choose **Add Data → Import JSON or CSV**.
5. Import the matching JSON file.

| JSON File | MongoDB Collection |
|---|---|
| `prices.json` | `prices` |
| `boyssuperdeluxerooms.json` | `boyssuperdeluxerooms` |
| `boysdeluxerooms.json` | `boysdeluxerooms` |
| `boysstandardrooms.json` | `boysstandardrooms` |
| `girlssuperdeluxerooms.json` | `girlssuperdeluxerooms` |
| `girlsdeluxerooms.json` | `girlsdeluxerooms` |
| `girlsstandardrooms.json` | `girlsstandardrooms` |

Import each file into its corresponding collection, not all into one collection.

### 3. Room availability

Room documents should contain the fields expected by the application, including:

- `roomNo` — room number
- `personNo` — room occupancy or capacity information
- `isStatus` — room availability status

The application filters out rooms where `isStatus` is `true`. If no rooms appear in the Add Student form, verify that the appropriate room collections contain documents with the expected field values.

## Running the Application

The frontend and backend run separately. Keep both terminal windows open while using the application.

### 1. Start the backend

Open a terminal and navigate to the application directory:

```bash
cd hostelManagement
npm run server
```

Alternatively, run:

```bash
npm run start:server
```

The backend uses port `4050` by default.

Verify that the server starts and connects to MongoDB before testing features that use the API.

### 2. Start the Angular frontend

Open a second terminal in the same application directory:

```bash
cd hostelManagement
npm start
```

If this older Angular/Webpack project encounters `ERR_OSSL_EVP_UNSUPPORTED` with a newer Node.js version, try the following in PowerShell:

```powershell
$env:NODE_OPTIONS="--openssl-legacy-provider"
npm start
```

This environment variable applies only to the current PowerShell session.

### 3. Open the application

When Angular starts successfully, open:

```text
http://localhost:4200
```

If port `4200` is already in use, Angular may ask to use another port. If you accept, use the URL and port displayed in the terminal.

## Admin Account Setup

Public registration creates a regular account with the role `User`. The application does not provide a default admin login.

To configure your own account for admin access:

1. Start MongoDB, the backend, and the frontend.
2. Open `http://localhost:4200/auth/SignUp`.
3. Register an account that you control.
4. Open MongoDB Compass and select `hostel` → `users`.
5. Find your account and change its `role` field from `User` to `Admin`.
6. Save the document.
7. Sign out of the application and sign in again.
8. Open the admin dashboard at `http://localhost:4200/admin/dashboard`.

Only change the role of an account you own or are authorized to administer. For a deployed application, use a secure, authorized admin-provisioning process instead of exposing unrestricted role changes.

## Application Routes

The following URLs assume that Angular is running on port `4200`.

### Public Pages

| Page | Route |
|---|---|
| Dashboard | `/dashboard` |
| Sign In | `/auth/SignIn` |
| Sign Up | `/auth/SignUp` |
| Hostel Details | `/hostel-detail` |
| Contact Us | `/contactUs` |
| Profile | `/profile` |

### Admin Pages

Sign in with an account that has the `Admin` role.

| Feature | Route |
|---|---|
| Admin Dashboard | `/admin/dashboard` |
| Add Student | `/admin/addStudent` |
| View Students | `/admin/viewStudent` |
| Update Student | `/admin/updateStudent` |
| Food Menu | `/admin/foodMenu` |
| Inquiries | `/admin/inquiry` |
| Price Details | `/admin/priceDetails` |
| Users | `/admin/users` |
| Room Structure | `/admin/roomStructure` |

If a route does not open, verify its exact path in the relevant Angular routing module.

### Adding Students

To add a student:

1. Open the Add Student page.
2. Select the student's gender and room category.
3. Choose an available room from the Room No list.
4. Complete the form and submit it.

The room list depends on the matching MongoDB collection and the room availability status.

## Troubleshooting

### `npm` cannot find `package.json`

Make sure the terminal is inside the `hostelManagement` directory:

```powershell
cd D:\AngularProjects\HostelManagementSystem-main\hostelManagement
```

Run npm commands from this directory.

### Angular error: `ERR_OSSL_EVP_UNSUPPORTED`

In PowerShell, run:

```powershell
$env:NODE_OPTIONS="--openssl-legacy-provider"
npm start
```

### Port 4200 is already in use

Use the existing Angular server if it is running, or start Angular on another port:

```bash
npm start -- --port 4201
```

Then open `http://localhost:4201`.

### Backend cannot connect to MongoDB

1. Confirm that MongoDB Community Server is installed and running.
2. Verify the `MONGODB_URI` value in `.env`.
3. For a local setup, check that the URI is:

   ```text
   mongodb://localhost:27017/hostel
   ```

4. Confirm that MongoDB is listening on port `27017`.
5. Restart the backend after correcting the configuration.

### Prices page returns an error

Confirm that the `prices` collection exists in the `hostel` database and contains the expected documents. Import `prices.json` if the sample data is missing.

### Add Student does not show room numbers

Check that all six room JSON files have been imported into their respective collections. Verify the expected document fields and ensure that available rooms have the appropriate `isStatus` value.

### Admin pages redirect or are unavailable

Confirm that your account's `role` is `Admin` in `hostel.users`. Sign out and sign in again after updating the role. Also check the route configuration and backend authorization logic.

### Profile image returns 404

Check that the requested image exists in the frontend assets directory and that the image path referenced by the application is correct.

## Available Commands

Run these commands from the `hostelManagement` directory.

| Command | Description |
|---|---|
| `npm install` | Install project dependencies |
| `npm start` | Start the Angular frontend |
| `npm run server` | Start the backend using its configured script |
| `npm run start:server` | Start the backend using Node.js |
| `npm run build` | Build the Angular application |
| `npm test` | Run Angular unit tests |

Check `package.json` if a command is unavailable or behaves differently in your local setup.

## Notes

- MongoDB, the backend, and the frontend must be running for database-dependent features to work.
- Keep private credentials and `.env` files out of version control.
- Import sample data before testing room availability and pricing features.
- The routes and scripts documented here should be checked against the current project configuration if the application is modified.

---

**Developed using Angular, Node.js, Express.js, and MongoDB.**
