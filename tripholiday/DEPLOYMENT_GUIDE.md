# Trip Holiday - Complete Deployment Guide

## Table of Contents
1. [Pre-Deployment Checklist](#pre-deployment-checklist)
2. [Database Setup (MongoDB Atlas)](#database-setup-mongodb-atlas)
3. [Backend Deployment](#backend-deployment)
   - [Option 1: Render (Recommended)](#option-1-render-recommended)
   - [Option 2: Railway](#option-2-railway)
   - [Option 3: Heroku](#option-3-heroku)
   - [Option 4: DigitalOcean/VPS](#option-4-digitalocean-vps)
4. [Frontend Deployment](#frontend-deployment)
   - [Option 1: Vercel (Recommended)](#option-1-vercel-recommended)
   - [Option 2: Netlify](#option-2-netlify)
   - [Option 3: GitHub Pages](#option-3-github-pages)
5. [Full Stack Deployment (Single Platform)](#full-stack-deployment)
6. [Post-Deployment Configuration](#post-deployment-configuration)
7. [Domain Configuration](#domain-configuration)
8. [Troubleshooting](#troubleshooting)

---

## Pre-Deployment Checklist

### 1. Prepare Your Project

```bash
# Navigate to your project
cd "C:\Users\BALA\Desktop\travel web\tripholiday"

# Ensure all dependencies are listed
cd backend
npm install

# Test locally
npm run dev
```

### 2. Update Configuration Files

**Create production-ready `.env` file:**
```env
PORT=5000
NODE_ENV=production
MONGODB_URI=your-mongodb-atlas-uri
JWT_SECRET=your-super-secure-random-secret-key
JWT_EXPIRE=7d
CORS_ORIGIN=https://your-frontend-domain.com
```

### 3. Update Frontend API URLs

**In all frontend HTML files**, update API endpoints from `localhost:5000` to your production backend URL:

```javascript
// OLD (Development)
const API_URL = 'http://localhost:5000/api';

// NEW (Production)
const API_URL = 'https://your-backend-url.com/api';
```

Files to update:
- `auth.js`
- `script.js`
- `packages.html`
- `profile.html`
- Any file making API calls

### 4. Security Checklist
- [ ] Generate strong JWT_SECRET (use: `node -e "console.log(require('crypto').randomBytes(64).toString('hex'))"`)
- [ ] Never commit `.env` file to Git
- [ ] Ensure `.gitignore` includes `node_modules/`, `.env`, `uploads/`
- [ ] Enable HTTPS on production
- [ ] Set secure CORS origins

---

## Database Setup (MongoDB Atlas)

### Step 1: Create MongoDB Atlas Account

1. Go to [https://www.mongodb.com/cloud/atlas](https://www.mongodb.com/cloud/atlas)
2. Sign up for a free account
3. Create a new cluster (Free tier: M0)

### Step 2: Configure Database

1. **Create Cluster:**
   - Choose Cloud Provider (AWS recommended)
   - Select Region (closest to your users)
   - Cluster Tier: M0 Sandbox (Free)
   - Click "Create Cluster"

2. **Setup Database Access:**
   - Go to "Database Access"
   - Click "Add New Database User"
   - Username: `tripholiday_admin`
   - Password: Generate secure password (save it!)
   - Database User Privileges: "Read and write to any database"
   - Click "Add User"

3. **Setup Network Access:**
   - Go to "Network Access"
   - Click "Add IP Address"
   - Select "Allow Access from Anywhere" (0.0.0.0/0)
   - Or add specific IPs of your hosting servers
   - Click "Confirm"

4. **Get Connection String:**
   - Go to "Clusters"
   - Click "Connect" on your cluster
   - Choose "Connect your application"
   - Copy the connection string:
   ```
   mongodb+srv://tripholiday_admin:<password>@cluster0.xxxxx.mongodb.net/tripholiday?retryWrites=true&w=majority
   ```
   - Replace `<password>` with your actual password
   - Replace `tripholiday` with your database name

### Step 3: Seed Database

```bash
# Update .env with MongoDB Atlas URI
# Then run:
npm run seed
```

---

## Backend Deployment

### Option 1: Render (Recommended - Free)

**Why Render?** Free tier, automatic deployments, easy setup

#### Steps:

1. **Prepare Backend:**
   ```bash
   cd backend
   # Ensure package.json has start script:
   # "start": "node server.js"
   ```

2. **Create Render Account:**
   - Go to [https://render.com](https://render.com)
   - Sign up with GitHub

3. **Create New Web Service:**
   - Dashboard → "New" → "Web Service"
   - Connect your GitHub repository
   - Select `tripholiday` repository

4. **Configure Service:**
   ```
   Name: tripholiday-api
   Region: Choose closest to your users
   Branch: main
   Root Directory: backend
   Runtime: Node
   Build Command: npm install
   Start Command: npm start
   ```

5. **Add Environment Variables:**
   Click "Advanced" → "Add Environment Variable":
   ```
   PORT=5000
   NODE_ENV=production
   MONGODB_URI=your-mongodb-atlas-connection-string
   JWT_SECRET=your-generated-secret
   JWT_EXPIRE=7d
   CORS_ORIGIN=https://your-frontend-url.vercel.app
   ```

6. **Deploy:**
   - Click "Create Web Service"
   - Wait for deployment (5-10 minutes)
   - Your API will be available at: `https://tripholiday-api.onrender.com`

7. **Note:** Free tier sleeps after 15 min of inactivity (first request may be slow)

---

### Option 2: Railway

**Why Railway?** Easy deployment, generous free tier

#### Steps:

1. **Create Railway Account:**
   - Go to [https://railway.app](https://railway.app)
   - Sign up with GitHub

2. **Create New Project:**
   - Dashboard → "New Project"
   - "Deploy from GitHub repo"
   - Select your repository

3. **Configure:**
   - Railway auto-detects Node.js
   - Root directory: `backend`
   - Build command: `npm install`
   - Start command: `npm start`

4. **Add Environment Variables:**
   - Settings → Variables
   - Add all variables from `.env`

5. **Deploy:**
   - Railway automatically deploys
   - Get your URL from Settings → Domains
   - Generate domain: `tripholiday-api.up.railway.app`

---

### Option 3: Heroku

**Note:** Heroku no longer offers free tier (starts at $7/month)

#### Steps:

1. **Install Heroku CLI:**
   ```bash
   # Download from: https://devcenter.heroku.com/articles/heroku-cli
   ```

2. **Login and Create App:**
   ```bash
   heroku login
   cd backend
   heroku create tripholiday-api
   ```

3. **Add MongoDB Atlas Add-on or use your Atlas URI:**
   ```bash
   heroku config:set MONGODB_URI="your-mongodb-atlas-uri"
   heroku config:set JWT_SECRET="your-secret"
   heroku config:set NODE_ENV=production
   heroku config:set CORS_ORIGIN="https://your-frontend.com"
   ```

4. **Deploy:**
   ```bash
   git push heroku main
   ```

5. **Your API:** `https://tripholiday-api.herokuapp.com`

---

### Option 4: DigitalOcean / VPS (Advanced)

**For production-grade deployment with full control**

#### Steps:

1. **Create Droplet:**
   - Choose Ubuntu 22.04 LTS
   - Basic plan: $6/month
   - Select datacenter region

2. **SSH into Server:**
   ```bash
   ssh root@your-server-ip
   ```

3. **Install Node.js:**
   ```bash
   curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
   sudo apt-get install -y nodejs
   sudo apt-get install -y nginx
   sudo apt-get install -y git
   ```

4. **Clone Repository:**
   ```bash
   cd /var/www
   git clone https://github.com/balamurugesan03/tripholiday.git
   cd tripholiday/backend
   npm install
   ```

5. **Setup Environment:**
   ```bash
   nano .env
   # Add all environment variables
   ```

6. **Install PM2 (Process Manager):**
   ```bash
   sudo npm install -g pm2
   pm2 start server.js --name tripholiday-api
   pm2 startup
   pm2 save
   ```

7. **Configure Nginx:**
   ```bash
   sudo nano /etc/nginx/sites-available/tripholiday
   ```

   Add:
   ```nginx
   server {
       listen 80;
       server_name your-domain.com;

       location / {
           proxy_pass http://localhost:5000;
           proxy_http_version 1.1;
           proxy_set_header Upgrade $http_upgrade;
           proxy_set_header Connection 'upgrade';
           proxy_set_header Host $host;
           proxy_cache_bypass $http_upgrade;
       }
   }
   ```

   ```bash
   sudo ln -s /etc/nginx/sites-available/tripholiday /etc/nginx/sites-enabled/
   sudo nginx -t
   sudo systemctl restart nginx
   ```

8. **Setup SSL (Free with Let's Encrypt):**
   ```bash
   sudo apt-get install certbot python3-certbot-nginx
   sudo certbot --nginx -d your-domain.com
   ```

---

## Frontend Deployment

### Option 1: Vercel (Recommended - Free)

**Why Vercel?** Fast CDN, automatic HTTPS, easy deployment

#### Steps:

1. **Install Vercel CLI (Optional):**
   ```bash
   npm install -g vercel
   ```

2. **Prepare Frontend:**
   - Create `vercel.json` in root:
   ```json
   {
     "version": 2,
     "builds": [
       {
         "src": "*.html",
         "use": "@vercel/static"
       }
     ],
     "routes": [
       {
         "src": "/(.*)",
         "dest": "/$1"
       }
     ]
   }
   ```

3. **Deploy via Vercel Website:**
   - Go to [https://vercel.com](https://vercel.com)
   - Sign up with GitHub
   - "New Project" → Import your repository
   - Root Directory: `tripholiday` (or leave blank if frontend is in root)
   - Click "Deploy"

4. **Configure:**
   - Your site will be at: `https://tripholiday.vercel.app`
   - Add custom domain in Settings if you have one

5. **Update API URLs:**
   - After backend is deployed, update all API calls in frontend
   - Redeploy

---

### Option 2: Netlify

**Similar to Vercel, great for static sites**

#### Steps:

1. **Create `netlify.toml`:**
   ```toml
   [build]
     publish = "."
     command = "echo 'No build needed'"

   [[redirects]]
     from = "/*"
     to = "/index.html"
     status = 200
   ```

2. **Deploy:**
   - Go to [https://netlify.com](https://netlify.com)
   - "Add new site" → "Import from Git"
   - Connect GitHub repository
   - Build settings: Leave default
   - Deploy

3. **Your site:** `https://tripholiday.netlify.app`

---

### Option 3: GitHub Pages

**Free but limited (no server-side code)**

#### Steps:

1. **Prepare Repository:**
   ```bash
   # Move all frontend files to root or create docs/ folder
   ```

2. **Enable GitHub Pages:**
   - Go to repository Settings
   - Pages section
   - Source: main branch / (root)
   - Save

3. **Access:** `https://balamurugesan03.github.io/tripholiday/`

4. **Note:** GitHub Pages doesn't support server-side code, only static files

---

## Full Stack Deployment (Single Platform)

### Using Render (Frontend + Backend)

1. **Deploy Backend** (as shown above)

2. **Deploy Frontend:**
   - Render → "New" → "Static Site"
   - Connect repository
   - Build Command: (leave empty)
   - Publish Directory: `.` or `tripholiday`
   - Add environment variable:
     ```
     API_URL=https://tripholiday-api.onrender.com
     ```

3. **Configure CORS:**
   - Update backend `.env`:
     ```
     CORS_ORIGIN=https://tripholiday.onrender.com
     ```

---

## Post-Deployment Configuration

### 1. Update Environment Variables

**Backend Environment Variables:**
```env
PORT=5000
NODE_ENV=production
MONGODB_URI=mongodb+srv://user:pass@cluster.mongodb.net/tripholiday
JWT_SECRET=generated-super-secret-key-64-chars
JWT_EXPIRE=7d
CORS_ORIGIN=https://your-frontend-domain.com
```

### 2. Update Frontend API Configuration

**Create `config.js` in frontend:**
```javascript
const CONFIG = {
    API_URL: 'https://tripholiday-api.onrender.com/api',
    ADMIN_API_URL: 'https://tripholiday-api.onrender.com/api/admin',
    AUTH_API_URL: 'https://tripholiday-api.onrender.com/api/user/auth'
};
```

Include in HTML before other scripts:
```html
<script src="config.js"></script>
```

Update all fetch calls to use `CONFIG.API_URL`

### 3. Test Deployment

1. **Test Backend API:**
   ```bash
   curl https://your-backend-url.com/api/packages
   ```

2. **Test Frontend:**
   - Visit your frontend URL
   - Test all pages
   - Test authentication
   - Test package browsing

3. **Test Integration:**
   - Register new user
   - Login
   - Browse packages
   - Make test booking

### 4. Monitor and Maintain

- Set up error logging (use Sentry or LogRocket)
- Monitor uptime (use UptimeRobot)
- Set up backups for MongoDB
- Monitor API usage

---

## Domain Configuration

### Using Custom Domain

1. **Purchase Domain** (Namecheap, GoDaddy, etc.)

2. **Configure DNS:**

   **For Frontend (Vercel/Netlify):**
   ```
   Type: CNAME
   Name: www
   Value: cname.vercel-dns.com (or netlify.app)

   Type: A
   Name: @
   Value: [Get from Vercel/Netlify dashboard]
   ```

   **For Backend (Render):**
   ```
   Type: CNAME
   Name: api
   Value: your-service.onrender.com
   ```

3. **Update CORS:**
   ```env
   CORS_ORIGIN=https://tripholiday.com,https://www.tripholiday.com
   ```

4. **SSL Certificate:**
   - Vercel/Netlify: Automatic
   - Render: Automatic
   - VPS: Use Let's Encrypt

---

## Troubleshooting

### Common Issues:

#### 1. CORS Errors
**Solution:** Update backend CORS_ORIGIN to match frontend URL exactly

#### 2. API Returns 404
**Solution:**
- Check backend is deployed correctly
- Verify API routes in server.js
- Check frontend API_URL configuration

#### 3. Database Connection Failed
**Solution:**
- Verify MongoDB Atlas connection string
- Check IP whitelist in Atlas (0.0.0.0/0 for all)
- Ensure database user has correct permissions

#### 4. Images Not Loading
**Solution:**
- Use absolute URLs for images
- Configure static file serving in Express
- Use CDN for images (Cloudinary, AWS S3)

#### 5. Authentication Not Working
**Solution:**
- Check JWT_SECRET is set
- Verify CORS credentials are enabled
- Check token expiration

#### 6. Slow Cold Starts (Render Free Tier)
**Solution:**
- Use UptimeRobot to ping every 10 minutes
- Upgrade to paid tier
- Use Railway or Render paid tier

---

## Recommended Deployment Stack

### For Small Projects (Free):
- **Frontend:** Vercel
- **Backend:** Render
- **Database:** MongoDB Atlas (Free tier)
- **Cost:** $0/month

### For Production (Paid):
- **Frontend:** Vercel Pro ($20/month)
- **Backend:** Railway Pro ($5/month)
- **Database:** MongoDB Atlas M10 ($57/month)
- **CDN:** Cloudflare (Free)
- **Monitoring:** Sentry (Free tier)
- **Cost:** ~$82/month

### For Full Control:
- **Server:** DigitalOcean Droplet ($12/month)
- **Database:** MongoDB Atlas M10 ($57/month)
- **CDN:** Cloudflare (Free)
- **Domain:** Namecheap ($10/year)
- **Cost:** ~$70/month

---

## Quick Start Deployment (5 Minutes)

1. **Database:** Create MongoDB Atlas cluster → Get connection string
2. **Backend:** Deploy to Render → Add env variables → Get backend URL
3. **Frontend:** Update API URLs → Deploy to Vercel
4. **Test:** Visit frontend URL → Test functionality

---

## Support

For deployment issues:
- Check platform documentation
- Review error logs in deployment dashboard
- Test locally first
- Ensure all environment variables are set

Good luck with your deployment! 🚀
