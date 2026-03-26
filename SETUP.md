# LAZARUS Setup Guide

Complete step-by-step instructions for setting up the LAZARUS Medical Forensic Recovery Platform.

## Table of Contents
1. [System Requirements](#system-requirements)
2. [Choose Your Setup Method](#choose-your-setup-method)
3. [Option 1: Manual Setup (Recommended for Development)](#option-1-manual-setup)
4. [Option 2: Docker Setup](#option-2-docker-setup)
5. [Verification Checklist](#verification-checklist)
6. [Troubleshooting](#troubleshooting)

## System Requirements

### Minimum Requirements
- **RAM:** 4GB
- **Disk Space:** 2GB
- **OS:** Windows, macOS, or Linux

### Required Software

#### For Manual Setup:
- **Node.js 16+** — https://nodejs.org/
  - Includes npm
- **Python 3.8+** — https://www.python.org/
- **Git** (optional but recommended)

#### For Docker Setup:
- **Docker** — https://www.docker.com/
- **Docker Compose** (usually included with Docker Desktop)

## Choose Your Setup Method

### Development (Recommended):
Use **Option 1: Manual Setup** for hot-reload development and debugging.

### Quick Testing:
Use **Option 2: Docker Setup** for one-command startup.

### Production:
Configure environment variables and use Docker or your preferred deployment platform.

---

## Option 1: Manual Setup (Recommended for Development)

### Step 1: Clone/Download Project

```bash
# If you have the project as a zip
cd lazarus

# Or if cloning from git
git clone <repository-url>
cd lazarus
```

### Step 2: Backend Setup

#### 2.1 Navigate to backend directory
```bash
cd backend
```

#### 2.2 Create Python virtual environment
```bash
# On Windows
python -m venv venv
venv\Scripts\activate

# On macOS/Linux
python3 -m venv venv
source venv/bin/activate
```

#### 2.3 Install Python dependencies
```bash
pip install -r requirements.txt
```

Expected output:
```
Successfully installed fastapi uvicorn sqlalchemy pydantic ...
```

#### 2.4 Create environment file
```bash
# Copy the example .env file
cp .env.example .env

# On Windows
copy .env.example .env
```

Edit `.env` if needed (defaults are fine for development):
```
SECRET_KEY=your-secret-key-change-in-production-12345
DATABASE_URL=sqlite:///./lazarus.db
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=30
```

#### 2.5 Seed the database
```bash
python -m app.seed.seed_data
```

Expected output:
```
✓ Database seeded successfully!
  - Created 3 demo users
  - Created 12 demo patients
  - Created telemetry logs (60 records)
  - Created prescription audits (60+ records)
  - Created recovery alerts (8 records)
  - Created timeline events (9 records)

Default login credentials:
  Email: analyst@lazarus.health
  Password: analyst123
```

#### 2.6 Start the backend server
```bash
python -m uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

Expected output:
```
INFO:     Uvicorn running on http://0.0.0.0:8000
INFO:     Application startup complete
```

**Backend is now running at:**
- API: http://localhost:8000/api
- Interactive Docs: http://localhost:8000/docs
- Alternative Docs: http://localhost:8000/redoc

✅ **Keep this terminal running!**

### Step 3: Frontend Setup (Open New Terminal)

#### 3.1 Navigate to frontend directory
```bash
# From project root in NEW terminal
cd frontend
```

#### 3.2 Install Node dependencies
```bash
npm install
```

Expected output:
```
added 200+ packages in 60s
```

#### 3.3 Create environment file
```bash
# Copy the example .env file
cp .env.example .env

# On Windows
copy .env.example .env
```

Content should be:
```
VITE_API_URL=http://localhost:8000/api
```

#### 3.4 Start the frontend server
```bash
npm run dev
```

Expected output:
```
VITE v5.0.8  ready in 200 ms

➜  Local:   http://localhost:5173/
➜  press h to show help
```

✅ **Frontend is now running at:** http://localhost:5173

### Step 4: Access the Application

1. Open your browser
2. Go to **http://localhost:5173**
3. Login with:
   - **Email:** `analyst@lazarus.health`
   - **Password:** `analyst123`

---

## Option 2: Docker Setup

### Prerequisites
- Docker Desktop installed and running

### Step 1: Navigate to Project
```bash
cd lazarus
```

### Step 2: Build and Start Services
```bash
docker-compose up --build
```

First run will take 2-3 minutes to build images.

Expected output:
```
Creating lazarus_backend_1 ... done
Creating lazarus_frontend_1 ... done
Attaching to lazarus_backend_1, lazarus_frontend_1
...
backend_1    | INFO:     Uvicorn running on http://0.0.0.0:8000
frontend_1   | VITE v5.0.8  ready in 200 ms
```

### Step 3: Access the Application
- **Frontend:** http://localhost:5173
- **Backend API:** http://localhost:8000/api
- **API Docs:** http://localhost:8000/docs

### Step 4: Stop Services
```bash
# Press Ctrl+C in terminal, or in another terminal:
docker-compose down
```

### Notes on Docker Setup
- Changes to Python code require container restart
- Changes to frontend code auto-reload
- Database persists in `lazarus_db` volume
- For production, set `DEBUG=false` and update SECRET_KEY

---

## Verification Checklist

After setup, verify everything works:

### ✅ Backend Verification
- [ ] Backend running on port 8000
- [ ] `/health` endpoint returns `{"status": "healthy"}`
- [ ] `/docs` shows API documentation
- [ ] Database file created (`lazarus.db`)
- [ ] Demo users created in database

### ✅ Frontend Verification
- [ ] Frontend running on port 5173
- [ ] Login page loads at http://localhost:5173/login
- [ ] Can login with `analyst@lazarus.health` / `analyst123`
- [ ] Dashboard loads with KPI cards
- [ ] Sidebar navigation works
- [ ] Patient data displays

### ✅ Integration Verification
- [ ] API calls succeed (check browser Network tab)
- [ ] No CORS errors in console
- [ ] Animations and styling display correctly
- [ ] Responsive design works on mobile view

### Test the Complete Flow
```
1. Login → analyst@lazarus.health / analyst123
2. Navigate to Dashboard → See KPI cards
3. Go to Patients → See 12 patient records
4. Click patient → View details and telemetry chart
5. Check Alerts → View recovery alerts
6. View Timeline → See incident events
7. Access Telemetry → Monitor vital signs
8. Check Medications → View medication audit trails
```

---

## Troubleshooting

### Backend Issues

#### Error: "ModuleNotFoundError: No module named 'fastapi'"
**Solution:**
```bash
# Ensure virtual environment is activated
source venv/bin/activate  # macOS/Linux
venv\Scripts\activate     # Windows

# Reinstall dependencies
pip install -r requirements.txt
```

#### Error: "Address already in use port 8000"
**Solution:**
```bash
# Change port
python -m uvicorn app.main:app --reload --port 8001

# Or kill process on port 8000
# Windows:
netstat -ano | findstr :8000
taskkill /PID <PID> /F

# macOS/Linux:
lsof -ti:8000 | xargs kill -9
```

#### Error: "Database connection failed"
**Solution:**
```bash
# Remove old database and reseed
rm lazarus.db
python -m app.seed.seed_data
```

#### CORS Error: "Access to XMLHttpRequest blocked"
**Solution:**
- Ensure backend is running on 8000
- Check frontend `.env` has correct API URL
- Restart both servers

### Frontend Issues

#### Error: "npm: command not found"
**Solution:**
- Install Node.js from https://nodejs.org/
- Restart terminal
- Verify: `node --version && npm --version`

#### Error: "VITE v5... Module not found"
**Solution:**
```bash
# Delete and reinstall dependencies
rm -rf node_modules
npm install

# Or with yarn
yarn install
```

#### White screen or blank page
**Solution:**
- Open browser console (F12 → Console)
- Check for error messages
- Verify backend is running
- Check `VITE_API_URL` in `.env`
- Hard refresh: Ctrl+Shift+R (Windows) or Cmd+Shift+R (Mac)

#### Styling broken or components not displaying
**Solution:**
```bash
# Rebuild Tailwind
npm install
npm run dev
```

### Common Solutions

#### "Port already in use"
```bash
# Backend on different port
python -m uvicorn app.main:app --port 9000

# Frontend on different port
npm run dev -- --port 3000
```

#### Clear Browser Cache
1. Open DevTools (F12)
2. Settings → Clear Site Data
3. Hard refresh (Ctrl+Shift+R)

#### Reset Database
```bash
# Backend directory
rm lazarus.db
python -m app.seed.seed_data
```

#### Full Reset
```bash
# Completely clean restart
# Backend
rm -rf venv lazarus.db
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python -m app.seed.seed_data

# Frontend
rm -rf node_modules
npm install
```

---

## Login Test Users

### Analyst (Recommended)
- **Email:** `analyst@lazarus.health`
- **Password:** `analyst123`
- **Role:** Analyst
- **Access:** Full dashboard, patient records, all features

### Clinician
- **Email:** `clinician@lazarus.health`
- **Password:** `clinician123`
- **Role:** Clinician
- **Access:** Patient data, vitals, alerts

### Admin
- **Email:** `admin@lazarus.health`
- **Password:** `admin123`
- **Role:** Admin
- **Access:** All features + administration

---

## Demo Data Included

### Patients: 12 total
- 2 Critical (extreme vital signs)
- 2 Warning (elevated metrics)
- 8 Stable
- Full demographics, telemetry, medications

### Alerts: 8 total
- 3 Critical (immediate action needed)
- 2 Warning (review recommended)
- 3 Info (status updates)

### Events: 9 timeline events
- Disk snapshots
- Recovery milestones
- Decoding completions
- Alert escalations

---

## GPU/Performance Notes

The application is lightweight and works on any system.

- **Frontend:** Uses Vite (instant reloads)
- **Backend:** Single Python process
- **Database:** Embedded SQLite
- **Memory:** Typically <200MB total

No GPU required.

---

## Next Steps

After successful setup:

1. **Explore the Dashboard**
   - View KPI metrics
   - Check patient statistics
   - Review recovery status

2. **Review Patient Data**
   - Search and filter patients
   - View individual records
   - Check recovery scores

3. **Monitor Vitals**
   - View telemetry charts
   - Check alert status
   - Review medication records

4. **Customize**
   - Adjust color scheme in `tailwind.config.js`
   - Modify seed data in `app/seed/seed_data.py`
   - Add new API endpoints in `app/api/`

5. **Deploy**
   - See README.md for deployment instructions
   - Configure production environment variables
   - Set up database backups

---

## Support & Documentation

- **API Docs:** http://localhost:8000/docs
- **Code Structure:** See README.md
- **Troubleshooting:** See above

---

## Summary

| Component | Command | URL |
|-----------|---------|-----|
| Backend | `python -m uvicorn app.main:app --reload` | http://localhost:8000 |
| Frontend | `npm run dev` | http://localhost:5173 |
| API Docs | Auto (backend) | http://localhost:8000/docs |
| Database | SQLite (auto) | `./lazarus.db` |

**Total Setup Time:** 10-15 minutes (first run)

---

**LAZARUS — Medical Forensic Recovery Platform**

Ready to start? Begin with [Option 1: Manual Setup](#option-1-manual-setup)
