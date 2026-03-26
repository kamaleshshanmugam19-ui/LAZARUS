# ⚡ QUICKSTART — 5 Minutes to LAZARUS

## TL;DR — Get Running Now

### Terminal 1: Backend
```bash
cd backend
python -m venv venv
source venv/bin/activate          # Windows: venv\Scripts\activate
pip install -r requirements.txt
python -m app.seed.seed_data
python -m uvicorn app.main:app --reload
```

✅ Backend ready at: http://localhost:8000

### Terminal 2: Frontend  
```bash
cd frontend
npm install
npm run dev
```

✅ Frontend ready at: http://localhost:5173

### Terminal 3: Open Browser
```
http://localhost:5173
Login: analyst@lazarus.health / analyst123
```

**Done! 🎉**

---

## Docker Quickstart (1 Command)

```bash
docker-compose up --build
```

Then open: **http://localhost:5173**

---

## Default Credentials

| Role | Email | Password |
|------|-------|----------|
| Analyst | analyst@lazarus.health | analyst123 |
| Clinician | clinician@lazarus.health | clinician123 |
| Admin | admin@lazarus.health | admin123 |

---

## What You Get

✅ **12 patient records** - Complete demographics
✅ **60+ vital sign readings** - Real telemetry data  
✅ **60+ medications** - Prescription audit trails
✅ **8 alerts** - Critical, warning, and info
✅ **9 timeline events** - Recovery milestones
✅ **3 user accounts** - Different roles

---

## Key Features (Try These)

1. **Dashboard** - View KPI metrics
2. **Patients** - Search 12 patients, view details
3. **Telemetry** - See vital signs charts with critical alerts
4. **Medications** - View medication decode status
5. **Alerts** - Check recovery alerts (3 critical)
6. **Timeline** - See incident recovery milestones

---

## Troubleshooting

| Issue | Fix |
|-------|-----|
| Port 8000 in use | `lsof -ti:8000 \| xargs kill -9` |
| Port 5173 in use | `npm run dev -- --port 3000` |
| Python venv error | Delete `venv/` folder, recreate |
| npm error | Delete `node_modules/`, run `npm install` |
| No data | Run `python -m app.seed.seed_data` |

---

## API Documentation

- **Interactive Docs:** http://localhost:8000/docs
- **Alternative Docs:** http://localhost:8000/redoc

---

## Project Structure

```
lazarus/
├── backend/          # Python FastAPI
├── frontend/         # React + Vite
├── README.md         # Full documentation
├── SETUP.md          # Detailed setup guide
└── API_REFERENCE.md  # All endpoints
```

---

## Next Steps

- 📘 Read [SETUP.md](SETUP.md) for detailed instructions
- 📖 Read [README.md](README.md) for features & architecture
- 🔌 Read [API_REFERENCE.md](API_REFERENCE.md) for all endpoints

---

## Tech Stack

**Frontend:** React 18, Vite, Tailwind CSS, Recharts
**Backend:** FastAPI, SQLAlchemy, SQLite, JWT
**Deployment:** Docker, Docker Compose

---

**Need help?** Check [SETUP.md](SETUP.md) Troubleshooting section

---

**LAZARUS — Medical Forensic Recovery Platform**
