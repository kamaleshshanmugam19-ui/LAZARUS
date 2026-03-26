# LAZARUS — Medical Forensic Recovery Platform

A premium, modern, full-stack medical forensic recovery dashboard designed for hospital incident response teams handling ransomware attacks. The platform reconstructs corrupted patient records, replays vital telemetry, decodes medication audit trails, and flags critical recovery risks.

## 🏥 Core Features

### 1. **User Authentication**
- Role-based access control (Admin, Analyst, Clinician)
- JWT-based authentication
- Secure password hashing with bcrypt
- Protected dashboard routes

### 2. **Dashboard**
- Real-time KPI cards with animated metrics
- Total patients recovered
- Identity reconstruction confidence
- Critical telemetry alerts
- Medication decode success rate
- Active recovery sessions
- Dataset integrity scoring

### 3. **Patient Management**
- Comprehensive patient demographics view
- Recovery confidence scoring
- Corruption tracking
- Reconstruction status (Stable/Warning/Critical)
- Advanced search and filtering
- Patient detail views with full history

### 4. **Vital Signs Monitoring**
- Real-time telemetry visualization
- Heart rate (BPM) monitoring with critical alerts
- Oxygen saturation (SpO2) tracking
- Temperature readings
- Historical trend charts
- Risk classification (Critical/Warning/Stable)
- Animated critical alerts with pulsing effects

### 5. **Medication Management**
- Prescription audit trail decoding
- Medication recovery status tracking
- Obfuscated medication resolution
- Dosage and frequency reconstruction
- Integrity scoring per medication
- Corrupted record flagging

### 6. **Recovery Alerts**
- Real-time alert system
- Multiple severity levels (Critical/Warning/Info)
- Alert categories (Telemetry, Identity Mismatch, Medication, Integrity)
- Read/unread state management
- Unread count badge in navigation

### 7. **Forensic Timeline**
- Incident event tracking
- Recovery milestone visualization
- Event severity color-coding
- Chronological event display
- Integration with recovery stages

### 8. **Recovery Scoring Engine**
- Intelligent confidence calculation
- Multi-factor scoring:
  - Demographic corruption percentage
  - Telemetry continuity
  - Medication decode completeness
  - Checksum health
  - Cross-dataset consistency
- High/Medium/Low confidence labels

## 🏗️ Architecture

### Frontend Stack
- **React 18** with Vite
- **React Router** for navigation
- **Tailwind CSS** for styling
- **Recharts** for data visualization
- **Lucide React** for icons
- **Framer Motion** for animations
- **Axios** for API communication

### Backend Stack
- **Python FastAPI** framework
- **SQLAlchemy** ORM
- **Pydantic** for data validation
- **SQLite** database (PostgreSQL-ready)
- **JWT** for authentication
- **Passlib + bcrypt** for password security
- **CORS** support

## 📁 Project Structure

```
lazarus/
├── backend/                          # Python FastAPI backend
│   ├── app/
│   │   ├── main.py                  # FastAPI application entry
│   │   ├── core/
│   │   │   ├── config.py            # Configuration settings
│   │   │   └── security.py          # JWT and password utilities
│   │   ├── db/
│   │   │   └── session.py           # Database session management
│   │   ├── models/
│   │   │   └── models.py            # SQLAlchemy models
│   │   ├── schemas/
│   │   │   └── schemas.py           # Pydantic request/response models
│   │   ├── api/
│   │   │   ├── auth.py              # Authentication endpoints
│   │   │   ├── patients.py          # Patient endpoints
│   │   │   ├── telemetry.py         # Telemetry endpoints
│   │   │   ├── medications.py       # Medication endpoints
│   │   │   ├── alerts.py            # Alert endpoints
│   │   │   └── dashboard.py         # Dashboard & timeline endpoints
│   │   ├── services/
│   │   │   └── recovery_service.py  # Business logic & recovery engine
│   │   └── seed/
│   │       └── seed_data.py         # Demo data seeding
│   ├── requirements.txt              # Python dependencies
│   └── .env.example                  # Environment variables template
│
├── frontend/                         # React + Vite frontend
│   ├── src/
│   │   ├── main.jsx                 # React entry point
│   │   ├── App.jsx                  # Main app component with routing
│   │   ├── index.css                # Global styles
│   │   ├── components/
│   │   │   ├── ProtectedRoute.jsx   # Route protection
│   │   │   ├── LoadingSpinner.jsx   # Loading state
│   │   │   ├── EmptyState.jsx       # Empty state UI
│   │   │   ├── StatCard.jsx         # KPI card component
│   │   │   ├── AlertBanner.jsx      # Alert notification
│   │   │   ├── Sidebar.jsx          # Navigation sidebar
│   │   │   └── TopNavbar.jsx        # Top navigation bar
│   │   ├── pages/
│   │   │   ├── LoginPage.jsx        # Login page
│   │   │   ├── RegisterPage.jsx     # Registration page
│   │   │   ├── DashboardPage.jsx    # Main dashboard
│   │   │   ├── PatientsPage.jsx     # Patient list
│   │   │   ├── PatientDetailPage.jsx # Patient details
│   │   │   ├── TelemetryPage.jsx    # Vital signs monitoring
│   │   │   ├── MedicationsPage.jsx  # Medication audit trail
│   │   │   ├── AlertsPage.jsx       # Recovery alerts
│   │   │   ├── TimelinePage.jsx     # Incident timeline
│   │   │   └── SettingsPage.jsx     # User settings
│   │   ├── layouts/
│   │   │   └── DashboardLayout.jsx  # Main layout wrapper
│   │   ├── context/
│   │   │   └── AuthContext.jsx      # Authentication state
│   │   ├── services/
│   │   │   └── api.js               # API service layer
│   │   ├── hooks/
│   │   │   └── useData.js           # Custom data fetching hooks
│   │   └── utils/                   # Utility functions
│   ├── package.json                 # Node dependencies
│   ├── vite.config.js               # Vite configuration
│   ├── tailwind.config.js           # Tailwind configuration
│   ├── postcss.config.js            # PostCSS configuration
│   ├── index.html                   # HTML entry point
│   └── .env.example                 # Environment variables template
│
└── README.md                         # This file
```

## 🚀 Quick Start

### Prerequisites
- **Node.js 16+** and **npm**
- **Python 3.8+** and **pip**
- **Git**

### Backend Setup

1. **Navigate to backend directory:**
   ```bash
   cd backend
   ```

2. **Create a virtual environment:**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

4. **Create .env file:**
   ```bash
   cp .env.example .env
   ```

5. **Seed the database:**
   ```bash
   python -m app.seed.seed_data
   ```

6. **Run the backend:**
   ```bash
   python -m uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
   ```

   The backend will be available at `http://localhost:8000`
   - API docs: `http://localhost:8000/docs`
   - ReDoc: `http://localhost:8000/redoc`

### Frontend Setup

1. **Navigate to frontend directory:**
   ```bash
   cd frontend
   ```

2. **Install dependencies:**
   ```bash
   npm install
   ```

3. **Create .env file:**
   ```bash
   cp .env.example .env
   ```

4. **Run the development server:**
   ```bash
   npm run dev
   ```

   The frontend will be available at `http://localhost:5173`

## 🔐 Default Login Credentials

The demo database comes pre-seeded with three user accounts:

### Analyst Account (Recommended for demo)
- **Email:** `analyst@lazarus.health`
- **Password:** `analyst123`
- **Role:** Analyst

### Clinician Account
- **Email:** `clinician@lazarus.health`
- **Password:** `clinician123`
- **Role:** Clinician

### Admin Account
- **Email:** `admin@lazarus.health`
- **Password:** `admin123`
- **Role:** Admin

## 📊 API Endpoints

### Authentication
- `POST /api/auth/register` - Register new user
- `POST /api/auth/login` - Login user
- `GET /api/auth/me` - Get current user

### Patients
- `GET /api/patients` - List all patients
- `GET /api/patients/{patient_id}` - Get patient details
- `GET /api/patients/{patient_id}/recovery-score` - Get recovery score

### Telemetry
- `GET /api/telemetry/{patient_id}` - Get telemetry history
- `GET /api/telemetry/{patient_id}/latest` - Get latest vitals

### Medications
- `GET /api/medications/{patient_id}` - Get patient medications

### Alerts
- `GET /api/alerts` - List all alerts
- `GET /api/alerts/{alert_id}/read` - Mark alert as read
- `GET /api/alerts/unread/count` - Get unread count

### Timeline
- `GET /api/timeline` - Get incident events

### Dashboard
- `GET /api/dashboard/summary` - Get KPI summary
- `GET /api/dashboard/profile` - Get user profile

## 🎨 Design System

### Color Palette
- **Primary:** Cyan (`#00d9ff`)
- **Secondary:** Emerald (`#00ff88`)
- **Accent:** Amber (`#ffb800`)
- **Critical:** Red (`#ff1744`)
- **Background:** Deep Slate (`#030712`, `#0f172a`)

### UI Components
- **Glassmorphism** effect with 10px blur
- **Rounded corners** (2xl for cards, lg for inputs)
- **Soft gradients** for visual hierarchy
- **Pulsing animations** for critical alerts
- **Smooth transitions** (0.3s ease)
- **Premium spacing** and typography hierarchy

## 🔄 Data Model

### User
- id, full_name, email, password_hash, role, created_at, is_active

### PatientDemographic
- id, patient_id, first_name, last_name, age, sex, blood_type
- ward, room, partial_national_id, checksum_status
- corruption_score, recovery_confidence, reconstruction_status, last_updated

### TelemetryLog
- id, patient_id, timestamp, bpm, spo2, temperature, integrity_score

### PrescriptionAudit
- id, patient_id, med_code, obfuscated_name, decoded_name
- dosage, frequency, decode_status, integrity_score

### RecoveryAlert
- id, patient_id, title, description, severity, category
- is_read, created_at

### IncidentTimelineEvent
- id, title, description, severity, event_type, created_at

## 🧪 Demo Data

The seeded database includes:
- **12 patient records** with varied reconstruction states
- **60+ telemetry readings** (vital signs)
- **60+ medication records** with different decode statuses
- **8 recovery alerts** with various severity levels
- **9 incident timeline events**
- **3 user accounts** (analyst, clinician, admin)

Demo includes:
- 2 critical patients with extreme vital signs
- 2 warning patients with elevated metrics
- 6+ stable patients
- Multiple corrupted medication records
- Realistic forensic recovery timeline

## 🔧 Environment Variables

### Backend (.env)
```
SECRET_KEY=your-secret-key-change-in-production
DATABASE_URL=sqlite:///./lazarus.db
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=30
```

### Frontend (.env)
```
VITE_API_URL=http://localhost:8000/api
```

## 📝 Database

The application uses **SQLite** by default for easy development. To switch to **PostgreSQL** in production:

1. Update `DATABASE_URL` in `.env`:
   ```
   DATABASE_URL=postgresql://user:password@localhost/lazarus
   ```

2. Install psycopg2:
   ```bash
   pip install psycopg2-binary
   ```

3. Create database and run migrations (if applicable)

## 🚢 Deployment

### Backend Deployment (Heroku/Railway/Render)
1. Set environment variables on platform
2. Run migrations
3. Deploy with `Procfile`:
   ```
   web: uvicorn app.main:app --host 0.0.0.0 --port $PORT
   ```

### Frontend Deployment (Vercel/Netlify)
1. Set `VITE_API_URL` to production backend URL
2. Build: `npm run build`
3. Deploy `dist/` folder

## 🔒 Security

- Passwords hashed with bcrypt
- JWT tokens for authentication
- HTTP-only cookie options (can be enabled)
- CORS protection
- Input validation with Pydantic
- SQL injection protection via SQLAlchemy ORM

## 📱 Responsive Design

- Desktop: Full layout with sidebar
- Tablet: Responsive grid and navigation
- Mobile: Collapsed sidebar, adapted components
- All pages optimized for mobile viewing

## 🎯 Key Features Highlights

✅ Real-time vital signs monitoring with critical alerts
✅ Intelligent recovery confidence scoring
✅ Medication audit trail with decode status
✅ Advanced patient search and filtering
✅ Responsive dashboard layout
✅ Premium glassmorphism UI
✅ Dark theme optimized for medical environments
✅ Complete authentication system
✅ Forensic timeline visualization
✅ Multi-role access control

## 🛠️ Development Mode

### Watch Frontend Changes
```bash
cd frontend
npm run dev
```

### Watch Backend Changes
```bash
cd backend
python -m uvicorn app.main:app --reload
```

### API Documentation
- Navigate to `http://localhost:8000/docs` for Swagger UI
- Navigate to `http://localhost:8000/redoc` for ReDoc

## 📄 License

This is a demonstration project for portfolio purposes.

## 🤝 Support

For issues or questions:
1. Check the API documentation at `/docs`
2. Review error messages in browser console
3. Check backend logs for API errors

---

**LAZARUS** — Restoring Trust in Medical Data Recovery

Built with ❤️ | Medical Forensic Recovery Platform
