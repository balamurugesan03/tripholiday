# Trip Holiday Backend API & Admin Panel

Complete backend system for Trip Holiday travel website with REST API and admin dashboard.

## 🚀 Features

- **RESTful API** - GET packages, filter by categories
- **Admin Dashboard** - Full CRUD operations for package management
- **Authentication** - JWT-based admin authentication
- **MongoDB Database** - Flexible NoSQL database for packages
- **Real-time Updates** - Changes reflect immediately on frontend
- **Responsive UI** - Modern admin panel that works on all devices

## 📋 Prerequisites

Before you begin, ensure you have the following installed:

- **Node.js** (v14 or higher) - [Download](https://nodejs.org/)
- **MongoDB** (v4.4 or higher) - [Download](https://www.mongodb.com/try/download/community)
- **npm** or **yarn** - Comes with Node.js

## 🛠️ Installation

### 1. Install Dependencies

```bash
cd backend
npm install
```

### 2. Setup Environment Variables

Create a `.env` file in the backend directory:

```bash
copy .env.example .env
```

Edit `.env` and configure:

```env
# Server Configuration
PORT=5000
NODE_ENV=development

# MongoDB Configuration
MONGODB_URI=mongodb://localhost:27017/tripholiday

# JWT Secret (Change this!)
JWT_SECRET=your-super-secret-key-change-this

# Admin Credentials
ADMIN_USERNAME=admin
ADMIN_EMAIL=admin@tripholiday.com
ADMIN_PASSWORD=Admin@123
```

### 3. Start MongoDB

Make sure MongoDB is running on your system:

**Windows:**
```bash
mongod
```

**macOS/Linux:**
```bash
sudo systemctl start mongod
```

### 4. Seed Database

Populate the database with initial data:

```bash
npm run seed
```

This will:
- Create sample packages
- Create default admin user
- Set up database indexes

### 5. Start Server

```bash
# Development mode (with auto-reload)
npm run dev

# Production mode
npm start
```

Server will start on: `http://localhost:5000`

## 📍 API Endpoints

### Public Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/packages` | Get all active packages |
| GET | `/api/packages/:id` | Get single package |

### Authentication

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/auth/login` | Admin login |
| GET | `/api/auth/verify` | Verify JWT token |

**Login Request:**
```json
{
  "username": "admin",
  "password": "Admin@123"
}
```

**Login Response:**
```json
{
  "success": true,
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "admin": {
    "id": "...",
    "username": "admin",
    "email": "admin@tripholiday.com",
    "name": "Administrator",
    "role": "superadmin"
  }
}
```

### Admin Endpoints (Requires Authentication)

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/admin/packages` | Get all packages |
| GET | `/api/admin/packages/:id` | Get single package |
| POST | `/api/admin/packages` | Create new package |
| PUT | `/api/admin/packages/:id` | Update package |
| DELETE | `/api/admin/packages/:id` | Delete package |
| PATCH | `/api/admin/packages/:id/toggle` | Toggle active status |
| GET | `/api/admin/stats` | Dashboard statistics |

**Authentication Header:**
```
Authorization: Bearer <token>
```

## 🎨 Admin Panel

Access the admin dashboard at: `http://localhost:5000/admin/login.html`

**Default Credentials:**
- Username: `admin`
- Password: `Admin@123`

### Admin Features

- ✅ View all packages in table format
- ✅ Add new packages with complete details
- ✅ Edit existing packages
- ✅ Delete packages
- ✅ Toggle package active/inactive status
- ✅ Filter packages by type, destination, status
- ✅ Search packages
- ✅ View dashboard statistics
- ✅ Manage day-by-day itineraries

## 🔧 Frontend Integration

Update the frontend to use the API:

**In `packages.html`, update API_CONFIG:**
```javascript
const API_CONFIG = {
    baseURL: 'http://localhost:5000/api',
    endpoints: {
        packages: '/packages'
    }
};
```

## 📁 Project Structure

```
backend/
├── config/
│   └── database.js          # MongoDB connection
├── middleware/
│   └── auth.js              # JWT authentication
├── models/
│   ├── Package.js           # Package schema
│   └── Admin.js             # Admin schema
├── public/
│   └── admin/
│       ├── index.html       # Admin dashboard
│       ├── login.html       # Admin login
│       ├── styles.css       # Admin styles
│       └── app.js           # Admin JavaScript
├── routes/
│   ├── api.js               # Public API routes
│   ├── admin.js             # Admin API routes
│   └── auth.js              # Auth routes
├── .env.example             # Environment template
├── package.json             # Dependencies
├── seedDatabase.js          # Database seeder
├── server.js                # Express server
└── README.md                # This file
```

## 🗄️ Database Schema

### Package Model

```javascript
{
  id: String,              // Unique identifier (e.g., "dubai")
  title: String,           // Package title
  description: String,     // Short description
  image: String,           // Image URL
  price: Number,           // Price in INR
  duration: Number,        // Days
  durationText: String,    // e.g., "7 Days / 6 Nights"
  type: String,            // luxury | premium | budget
  destination: String,     // india | international | pilgrimage
  travel: String,          // family | couple | solo | group | buddy
  popular: Boolean,        // Featured package
  popularBadge: String,    // Badge text
  tags: [String],          // Array of tags
  active: Boolean,         // Show on website
  itinerary: {
    title: String,
    subtitle: String,
    days: [{
      day: Number,
      title: String,
      description: String,
      highlights: [String]
    }]
  }
}
```

## 🚀 Deployment

### Deploy to Production

1. **Set Environment to Production:**
```env
NODE_ENV=production
CORS_ORIGIN=https://yourdomain.com
```

2. **Use MongoDB Atlas (Cloud):**
```env
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/tripholiday
```

3. **Build and Deploy:**
```bash
npm install --production
npm start
```

### Deploy to Hosting Platforms

**Heroku:**
```bash
heroku create tripholiday-api
git push heroku main
```

**Vercel/Netlify:**
- Deploy as Node.js serverless functions

**VPS (DigitalOcean, AWS EC2):**
- Use PM2 for process management:
```bash
npm install -g pm2
pm2 start server.js --name tripholiday-api
pm2 save
pm2 startup
```

## 🔒 Security

- JWT tokens for authentication
- Password hashing with bcrypt
- CORS enabled for specific origins
- Input validation on all endpoints
- Rate limiting (recommended for production)

## 🧪 Testing

Test the API with curl or Postman:

```bash
# Get all packages
curl http://localhost:5000/api/packages

# Login
curl -X POST http://localhost:5000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"username":"admin","password":"Admin@123"}'

# Create package (with token)
curl -X POST http://localhost:5000/api/admin/packages \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -d @package.json
```

## 📝 Scripts

```json
{
  "start": "node server.js",           // Start production server
  "dev": "nodemon server.js",          // Start development server
  "seed": "node seedDatabase.js"       // Seed database
}
```

## 🐛 Troubleshooting

**MongoDB Connection Error:**
- Ensure MongoDB is running
- Check MONGODB_URI in .env
- Verify MongoDB port (default: 27017)

**Authentication Error:**
- Clear browser localStorage
- Re-login to get new token
- Check JWT_SECRET in .env

**CORS Error:**
- Update CORS_ORIGIN in .env
- Restart server after changes

## 📚 API Documentation

Full API documentation available at:
`http://localhost:5000/api/docs`

## 🤝 Support

For issues or questions:
- Check existing issues on GitHub
- Create new issue with details
- Contact: admin@tripholiday.com

## 📄 License

MIT License - feel free to use for your projects!

---

**Built with ❤️ for Trip Holiday**
