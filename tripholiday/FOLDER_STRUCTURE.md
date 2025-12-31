# Trip Holiday - Complete Folder Structure

## Overview
This document provides a complete overview of the Trip Holiday project structure.

```
tripholiday/
├── backend/                          # Backend API & Admin Panel
│   ├── config/                       # Configuration files
│   │   └── database.js              # MongoDB connection setup
│   │
│   ├── middleware/                   # Express middleware
│   │   └── auth.js                  # JWT authentication middleware
│   │
│   ├── models/                       # Mongoose data models
│   │   ├── Admin.js                 # Admin user model
│   │   ├── Package.js               # Travel package model
│   │   └── User.js                  # Customer user model
│   │
│   ├── public/                       # Static files for admin panel
│   │   └── admin/                   # Admin dashboard
│   │       ├── index.html           # Admin dashboard page
│   │       ├── login.html           # Admin login page
│   │       ├── app.js               # Admin panel JavaScript
│   │       └── styles.css           # Admin panel styles
│   │
│   ├── routes/                       # API route handlers
│   │   ├── admin.js                 # Admin CRUD operations
│   │   ├── api.js                   # Public API routes (packages)
│   │   ├── auth.js                  # Admin authentication routes
│   │   ├── userAuth.js              # User authentication routes
│   │   └── userProfile.js           # User profile management
│   │
│   ├── uploads/                      # Uploaded files (created at runtime)
│   │   └── packages/                # Package images
│   │
│   ├── .env                         # Environment variables (DO NOT COMMIT)
│   ├── .env.example                 # Environment variables template
│   ├── package.json                 # Backend dependencies
│   ├── package-lock.json            # Locked dependency versions
│   ├── server.js                    # Main server entry point
│   ├── seedDatabase.js              # Database seeding script
│   └── README.md                    # Backend documentation
│
├── frontend/                         # Frontend HTML/CSS/JS files
│   ├── index.html                   # Homepage
│   ├── packages.html                # All packages listing
│   ├── blog.html                    # Blog page
│   ├── login.html                   # User login page
│   ├── register.html                # User registration page
│   ├── profile.html                 # User profile page
│   │
│   ├── Holiday Pages/
│   │   ├── luxury-holiday.html      # Luxury packages
│   │   ├── india-holiday.html       # India packages
│   │   ├── international-holiday.html # International packages
│   │   └── pilgrimage.html          # Pilgrimage packages
│   │
│   ├── Booking Pages/
│   │   ├── booking-with-flight.html # Booking with flight
│   │   └── booking-without-flight.html # Booking without flight
│   │
│   ├── Policy Pages/
│   │   ├── terms.html               # Terms & conditions
│   │   ├── cancellation-policy.html # Cancellation policy
│   │   ├── date-change-policy.html  # Date change policy
│   │   └── visa.html                # Visa services
│   │
│   ├── Assets/
│   │   ├── styles.css               # Main stylesheet
│   │   ├── script.js                # Main JavaScript
│   │   ├── auth.js                  # Authentication JavaScript
│   │   ├── update-prices.js         # Price update utility
│   │   └── WhatsApp Image 2025-12-28 at 8.53.38 PM.jpeg # Logo
│   │
│   └── payment.html                 # Payment page
│
├── Documentation/
│   ├── README.md                    # Project overview
│   ├── UPDATES.md                   # Version history
│   ├── ALL-PAGES-UPDATED.md         # Page updates log
│   ├── API_DOCUMENTATION.md         # API documentation
│   ├── FOLDER_STRUCTURE.md          # This file
│   └── DEPLOYMENT_GUIDE.md          # Hosting guide
│
└── .gitignore                       # Git ignore rules

```

## File Descriptions

### Backend Files

#### Configuration
- **backend/config/database.js**: MongoDB connection configuration with error handling

#### Middleware
- **backend/middleware/auth.js**: JWT token verification for protected routes

#### Models
- **backend/models/Admin.js**: Admin schema (username, email, password, role)
- **backend/models/Package.js**: Package schema (name, price, destination, itinerary, etc.)
- **backend/models/User.js**: User schema (name, email, password, bookings)

#### Routes
- **backend/routes/api.js**: Public API endpoints (GET packages, search, filter)
- **backend/routes/admin.js**: Admin operations (CRUD for packages)
- **backend/routes/auth.js**: Admin authentication (login, register)
- **backend/routes/userAuth.js**: User authentication (login, register)
- **backend/routes/userProfile.js**: User profile management

#### Main Files
- **backend/server.js**: Express server setup, middleware, routes
- **backend/seedDatabase.js**: Initial database population script
- **backend/package.json**: Dependencies and scripts

### Frontend Files

#### Main Pages
- **index.html**: Homepage with hero section, categories, featured packages
- **packages.html**: All packages with filtering and search
- **blog.html**: Travel blog and articles
- **payment.html**: Payment processing page

#### Authentication Pages
- **login.html**: User login interface
- **register.html**: User registration form
- **profile.html**: User dashboard and profile

#### Category Pages
- **luxury-holiday.html**: Premium luxury packages
- **india-holiday.html**: Domestic India tour packages
- **international-holiday.html**: International destinations
- **pilgrimage.html**: Religious pilgrimage tours

#### Booking Pages
- **booking-with-flight.html**: Complete booking with flight tickets
- **booking-without-flight.html**: Package booking without flights

#### Policy Pages
- **terms.html**: Terms and conditions
- **cancellation-policy.html**: Cancellation and refund policy
- **date-change-policy.html**: Date modification policy
- **visa.html**: Visa assistance services

#### Assets
- **styles.css**: Global styles, responsive design, animations
- **script.js**: Interactive features, form validation, animations
- **auth.js**: Frontend authentication logic
- **update-prices.js**: Dynamic price calculator

## Technology Stack

### Backend
- **Node.js**: JavaScript runtime
- **Express.js**: Web framework
- **MongoDB**: NoSQL database
- **Mongoose**: MongoDB ODM
- **JWT**: Authentication tokens
- **bcryptjs**: Password hashing
- **Multer**: File upload handling
- **Morgan**: HTTP request logger
- **CORS**: Cross-origin resource sharing

### Frontend
- **HTML5**: Markup
- **CSS3**: Styling with animations
- **JavaScript (ES6+)**: Interactivity
- **Fetch API**: HTTP requests
- **Local Storage**: Client-side data

## Environment Variables

See `.env.example` for required environment variables:
- `PORT`: Server port (default: 5000)
- `MONGODB_URI`: Database connection string
- `JWT_SECRET`: Secret key for JWT tokens
- `CORS_ORIGIN`: Allowed frontend origin
- `ADMIN_*`: Default admin credentials

## Important Notes

1. **node_modules/**: Not included in repository (install via `npm install`)
2. **uploads/**: Created at runtime for file uploads
3. **.env**: Contains sensitive data, never commit to repository
4. **Frontend Integration**: Frontend files need to point to correct backend API URL
5. **Security**: Always use HTTPS in production, secure JWT secrets

## Directory Size Estimates

- **Backend (with node_modules)**: ~150-200 MB
- **Backend (without node_modules)**: ~100 KB
- **Frontend**: ~500 KB - 1 MB
- **Total (excluding node_modules)**: ~1-2 MB

## Next Steps

1. Review DEPLOYMENT_GUIDE.md for hosting instructions
2. Configure environment variables for your environment
3. Test locally before deploying to production
4. Set up MongoDB Atlas for cloud database
5. Deploy to hosting platform (Vercel, Netlify, Heroku, etc.)
