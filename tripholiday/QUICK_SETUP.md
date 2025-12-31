# Trip Holiday - Quick Setup Guide

## Local Development Setup (10 Minutes)

### Prerequisites
- Node.js 16+ installed ([Download](https://nodejs.org/))
- Git installed
- MongoDB installed locally OR MongoDB Atlas account
- Code editor (VS Code recommended)

---

## Step 1: Clone and Install

```bash
# Navigate to project
cd "C:\Users\BALA\Desktop\travel web\tripholiday"

# Install backend dependencies
cd backend
npm install

# Go back to root
cd ..
```

---

## Step 2: Configure Environment

### Create `.env` file in backend folder:

```bash
cd backend
copy .env.example .env
```

### Edit `.env` with your values:

```env
# Server Configuration
PORT=5000
NODE_ENV=development

# MongoDB Configuration
# Option 1: Local MongoDB
MONGODB_URI=mongodb://localhost:27017/tripholiday

# Option 2: MongoDB Atlas (Recommended)
# MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/tripholiday

# JWT Secret (Generate a secure random string)
JWT_SECRET=your-super-secret-jwt-key-change-this-in-production

# JWT Expiration
JWT_EXPIRE=7d

# Admin Default Credentials
ADMIN_USERNAME=admin
ADMIN_EMAIL=admin@tripholiday.com
ADMIN_PASSWORD=Admin@123
ADMIN_NAME=Administrator

# CORS Origin (Frontend URL)
CORS_ORIGIN=http://localhost:3000

# File Upload Settings
MAX_FILE_SIZE=5242880

# Rate Limiting
RATE_LIMIT_WINDOW=15
RATE_LIMIT_MAX_REQUESTS=100
```

### Generate Secure JWT Secret:

**Windows PowerShell:**
```powershell
node -e "console.log(require('crypto').randomBytes(64).toString('hex'))"
```

**Copy the output and paste it as JWT_SECRET value**

---

## Step 3: Setup Database

### Option A: MongoDB Atlas (Cloud - Recommended)

1. **Create Account:**
   - Go to [https://www.mongodb.com/cloud/atlas](https://www.mongodb.com/cloud/atlas)
   - Sign up for free

2. **Create Cluster:**
   - Click "Build a Database"
   - Choose "FREE" (M0)
   - Select cloud provider and region
   - Click "Create"

3. **Create Database User:**
   - Go to "Database Access"
   - Click "Add New Database User"
   - Username: `tripholiday_admin`
   - Password: [Generate strong password]
   - Privileges: "Read and write to any database"
   - Click "Add User"

4. **Whitelist IP:**
   - Go to "Network Access"
   - Click "Add IP Address"
   - Click "Allow Access from Anywhere" (0.0.0.0/0)
   - Click "Confirm"

5. **Get Connection String:**
   - Go to "Database" → "Connect"
   - Choose "Connect your application"
   - Copy connection string:
   ```
   mongodb+srv://tripholiday_admin:<password>@cluster0.xxxxx.mongodb.net/tripholiday
   ```
   - Replace `<password>` with your actual password
   - Update MONGODB_URI in `.env`

### Option B: Local MongoDB

1. **Install MongoDB:**
   - Download from [https://www.mongodb.com/try/download/community](https://www.mongodb.com/try/download/community)
   - Install and run MongoDB service

2. **Use local URI:**
   ```env
   MONGODB_URI=mongodb://localhost:27017/tripholiday
   ```

---

## Step 4: Seed Database

```bash
# In backend folder
npm run seed
```

**Expected Output:**
```
✓ Database Connected
✓ Seeded 1 admin user
✓ Seeded 24 packages
✓ Database seeding completed!
```

---

## Step 5: Start Backend Server

```bash
# Development mode (with auto-restart)
npm run dev

# Production mode
npm start
```

**Expected Output:**
```
Server running in development mode on port 5000
MongoDB Connected: cluster0.xxxxx.mongodb.net
```

**Test Backend:**
Open browser and visit:
- API Root: [http://localhost:5000](http://localhost:5000)
- Get Packages: [http://localhost:5000/api/packages](http://localhost:5000/api/packages)
- Admin Panel: [http://localhost:5000/admin](http://localhost:5000/admin)

---

## Step 6: Setup Frontend

### Update API URLs in Frontend Files:

**Method 1: Create config.js (Recommended)**

Create `config.js` in root directory:
```javascript
const CONFIG = {
    // For local development
    API_URL: 'http://localhost:5000/api',
    ADMIN_API_URL: 'http://localhost:5000/api/admin',
    AUTH_API_URL: 'http://localhost:5000/api/user/auth',

    // For production (uncomment when deploying)
    // API_URL: 'https://your-backend-url.com/api',
    // ADMIN_API_URL: 'https://your-backend-url.com/api/admin',
    // AUTH_API_URL: 'https://your-backend-url.com/api/user/auth',
};
```

**Add to HTML files (before other scripts):**
```html
<script src="config.js"></script>
<script src="auth.js"></script>
<script src="script.js"></script>
```

**Update fetch calls to use config:**
```javascript
// OLD
fetch('http://localhost:5000/api/packages')

// NEW
fetch(`${CONFIG.API_URL}/packages`)
```

### Serve Frontend:

**Option 1: Live Server (VS Code Extension)**
- Install "Live Server" extension
- Right-click `index.html` → "Open with Live Server"
- Opens at [http://127.0.0.1:5500](http://127.0.0.1:5500)

**Option 2: Python HTTP Server**
```bash
# Python 3
python -m http.server 3000

# Python 2
python -m SimpleHTTPServer 3000
```

**Option 3: Node HTTP Server**
```bash
npx http-server -p 3000
```

Visit: [http://localhost:3000](http://localhost:3000)

---

## Step 7: Test the Application

### Test Admin Panel:
1. Visit [http://localhost:5000/admin](http://localhost:5000/admin)
2. Login with credentials from `.env`:
   - Username: `admin`
   - Password: `Admin@123`
3. Try adding/editing packages

### Test Frontend:
1. Visit [http://localhost:3000](http://localhost:3000)
2. Browse packages
3. Test registration: Click "Sign Up"
4. Test login
5. Test profile page

---

## Common Issues and Solutions

### Issue 1: "Cannot connect to MongoDB"
**Solution:**
- Check MONGODB_URI is correct
- For Atlas: Verify IP whitelist includes your IP
- For Local: Ensure MongoDB service is running

### Issue 2: "Port 5000 already in use"
**Solution:**
```bash
# Windows - Kill process on port 5000
netstat -ano | findstr :5000
taskkill /PID <PID> /F

# Or change PORT in .env to 5001
```

### Issue 3: "CORS Error" in browser
**Solution:**
- Update CORS_ORIGIN in backend `.env`
- Ensure it matches your frontend URL exactly
- Restart backend server

### Issue 4: "JWT Secret required"
**Solution:**
- Generate and add JWT_SECRET to `.env`
- Restart backend server

### Issue 5: "Admin login fails"
**Solution:**
- Check admin credentials in `.env`
- Run `npm run seed` again to reset admin user

---

## Project URLs

### Local Development:
- **Frontend:** http://localhost:3000
- **Backend API:** http://localhost:5000
- **Admin Panel:** http://localhost:5000/admin
- **API Docs:** http://localhost:5000/api

### Production (After Deployment):
- **Frontend:** https://tripholiday.vercel.app
- **Backend API:** https://tripholiday-api.onrender.com
- **Admin Panel:** https://tripholiday-api.onrender.com/admin

---

## Next Steps

1. ✅ Setup complete - Test all features locally
2. 📚 Read `API_DOCUMENTATION.md` for API endpoints
3. 🚀 When ready to deploy, read `DEPLOYMENT_GUIDE.md`
4. 📝 Review `FOLDER_STRUCTURE.md` for project organization

---

## Development Workflow

### Daily Development:
```bash
# Terminal 1 - Backend
cd backend
npm run dev

# Terminal 2 - Frontend (if needed)
npx http-server -p 3000
```

### Before Committing:
```bash
# Check what changed
git status

# Add changes
git add .

# Commit with message
git commit -m "Your descriptive message"

# Push to GitHub
git push
```

### Testing:
- Test all CRUD operations in admin panel
- Test user registration and login
- Test package browsing and filtering
- Test mobile responsive design

---

## Environment Variables Checklist

Backend `.env` file must have:
- [x] PORT
- [x] NODE_ENV
- [x] MONGODB_URI
- [x] JWT_SECRET
- [x] JWT_EXPIRE
- [x] ADMIN_USERNAME
- [x] ADMIN_EMAIL
- [x] ADMIN_PASSWORD
- [x] CORS_ORIGIN

---

## Support Commands

### Backend:
```bash
# Install dependencies
npm install

# Start development server
npm run dev

# Start production server
npm start

# Seed database
npm run seed

# Check for errors
npm run test
```

### Git:
```bash
# Check status
git status

# View changes
git diff

# Create branch
git checkout -b feature-name

# Push to GitHub
git push origin main
```

---

## Additional Resources

- **MongoDB Atlas Guide:** https://docs.atlas.mongodb.com/
- **Express.js Docs:** https://expressjs.com/
- **JWT Guide:** https://jwt.io/introduction
- **Mongoose Docs:** https://mongoosejs.com/docs/

---

## Quick Reference

### Default Admin Credentials:
```
Username: admin
Email: admin@tripholiday.com
Password: Admin@123
```

### API Endpoints:
```
GET    /api/packages              # Get all packages
GET    /api/packages/:id          # Get single package
POST   /api/user/auth/register    # Register user
POST   /api/user/auth/login       # Login user
GET    /api/user/profile          # Get user profile
```

### Project Structure:
```
tripholiday/
├── backend/           # Node.js backend
├── *.html            # Frontend pages
├── styles.css        # Global styles
└── script.js         # Frontend logic
```

---

**Setup Complete! Happy Coding! 🎉**

For deployment instructions, see `DEPLOYMENT_GUIDE.md`
