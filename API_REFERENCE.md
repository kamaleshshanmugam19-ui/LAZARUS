# API Reference Guide

Complete API endpoint documentation for LAZARUS backend.

## Base URL
```
http://localhost:8000/api
```

## Authentication

All endpoints except `/auth/register` and `/auth/login` require authentication via JWT token.

### Headers
```
Authorization: Bearer <token>
Content-Type: application/json
```

---

## Authentication Endpoints

### Register User
**POST** `/auth/register`

Create a new user account.

**Request Body:**
```json
{
  "email": "user@example.com",
  "full_name": "John Doe",
  "password": "secure_password_123",
  "role": "analyst"
}
```

**Response:** `200 OK`
```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6I",
  "token_type": "bearer",
  "user": {
    "id": 1,
    "email": "user@example.com",
    "full_name": "John Doe",
    "role": "analyst",
    "created_at": "2024-03-26T10:30:00Z"
  }
}
```

### Login User
**POST** `/auth/login`

Authenticate user with email and password.

**Request Body:**
```json
{
  "email": "analyst@lazarus.health",
  "password": "analyst123"
}
```

**Response:** `200 OK`
```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6I",
  "token_type": "bearer",
  "user": {
    "id": 2,
    "email": "analyst@lazarus.health",
    "full_name": "James Patterson",
    "role": "analyst",
    "created_at": "2024-03-26T10:30:00Z"
  }
}
```

### Get Current User
**GET** `/auth/me`

Get logged-in user's information.

**Response:** `200 OK`
```json
{
  "id": 2,
  "email": "analyst@lazarus.health",
  "full_name": "James Patterson",
  "role": "analyst",
  "created_at": "2024-03-26T10:30:00Z"
}
```

---

## Patient Endpoints

### List All Patients
**GET** `/patients`

Retrieve all patient records.

**Query Parameters:**
- None

**Response:** `200 OK`
```json
[
  {
    "id": 1,
    "patient_id": "PAT-001-FRAG",
    "first_name": "Robert",
    "last_name": "Henderson",
    "age": 62,
    "sex": "M",
    "blood_type": "O+",
    "ward": "ICU",
    "room": "101",
    "partial_national_id": "***-**-5847",
    "checksum_status": "valid",
    "corruption_score": 5.0,
    "recovery_confidence": 95.0,
    "reconstruction_status": "Stable",
    "last_updated": "2024-03-26T10:30:00Z"
  }
]
```

### Get Patient Details
**GET** `/patients/{patient_id}`

Retrieve detailed information for a specific patient including telemetry and medications.

**Path Parameters:**
- `patient_id` (string): Patient ID (e.g., "PAT-001-FRAG")

**Response:** `200 OK`
```json
{
  "id": 1,
  "patient_id": "PAT-001-FRAG",
  "first_name": "Robert",
  "last_name": "Henderson",
  "age": 62,
  "sex": "M",
  "blood_type": "O+",
  "ward": "ICU",
  "room": "101",
  "partial_national_id": "***-**-5847",
  "checksum_status": "valid",
  "corruption_score": 5.0,
  "recovery_confidence": 95.0,
  "reconstruction_status": "Stable",
  "last_updated": "2024-03-26T10:30:00Z",
  "telemetry_logs": [
    {
      "id": 1,
      "patient_id": "PAT-001-FRAG",
      "timestamp": "2024-03-26T10:25:00Z",
      "bpm": 72,
      "spo2": 98.0,
      "temperature": 37.2,
      "integrity_score": 98.0
    }
  ],
  "prescription_audits": [
    {
      "id": 1,
      "patient_id": "PAT-001-FRAG",
      "med_code": "MED-001",
      "obfuscated_name": "Lisinopril",
      "decoded_name": "Lisinopril",
      "dosage": "10mg",
      "frequency": "Once daily",
      "decode_status": "Recovered",
      "integrity_score": 100.0
    }
  ],
  "alerts": [
    {
      "id": 1,
      "patient_id": "PAT-001-FRAG",
      "title": "Alert Title",
      "description": "Alert description",
      "severity": "Warning",
      "category": "TelemetryAlert",
      "is_read": false,
      "created_at": "2024-03-26T10:30:00Z"
    }
  ]
}
```

### Get Recovery Score
**GET** `/patients/{patient_id}/recovery-score`

Calculate intelligence recovery confidence score for a patient.

**Path Parameters:**
- `patient_id` (string): Patient ID

**Response:** `200 OK`
```json
{
  "patient_id": "PAT-001-FRAG",
  "score": 89.5,
  "confidence_label": "High",
  "demographic_integrity": 95.0,
  "telemetry_continuity": 94.2,
  "medication_completeness": 85.0,
  "checksum_health": 100.0,
  "cross_dataset_consistency": 91.67
}
```

---

## Telemetry Endpoints

### Get Patient Telemetry History
**GET** `/telemetry/{patient_id}`

Retrieve historical telemetry readings for a patient.

**Path Parameters:**
- `patient_id` (string): Patient ID

**Query Parameters:**
- `limit` (integer): Max records to return (default: 100)

**Response:** `200 OK`
```json
[
  {
    "id": 1,
    "patient_id": "PAT-001-FRAG",
    "timestamp": "2024-03-26T10:25:00Z",
    "bpm": 72,
    "spo2": 98.0,
    "temperature": 37.2,
    "integrity_score": 98.0
  },
  {
    "id": 2,
    "patient_id": "PAT-001-FRAG",
    "timestamp": "2024-03-26T10:20:00Z",
    "bpm": 71,
    "spo2": 98.5,
    "temperature": 37.1,
    "integrity_score": 99.0
  }
]
```

### Get Latest Telemetry
**GET** `/telemetry/{patient_id}/latest`

Retrieve the most recent telemetry reading for a patient.

**Path Parameters:**
- `patient_id` (string): Patient ID

**Response:** `200 OK`
```json
{
  "bpm": 72,
  "spo2": 98.0,
  "temperature": 37.2,
  "status": "Stable",
  "last_timestamp": "2024-03-26T10:25:00Z"
}
```

---

## Medication Endpoints

### Get Patient Medications
**GET** `/medications/{patient_id}`

Retrieve medication audit trail for a patient.

**Path Parameters:**
- `patient_id` (string): Patient ID

**Response:** `200 OK`
```json
[
  {
    "id": 1,
    "patient_id": "PAT-001-FRAG",
    "med_code": "MED-001",
    "obfuscated_name": "Lisinopril",
    "decoded_name": "Lisinopril",
    "dosage": "10mg",
    "frequency": "Once daily",
    "decode_status": "Recovered",
    "integrity_score": 100.0
  },
  {
    "id": 2,
    "patient_id": "PAT-001-FRAG",
    "med_code": "MED-002",
    "obfuscated_name": "Metoprolol",
    "decoded_name": "Metoprolol tartrate",
    "dosage": "25mg",
    "frequency": "Twice daily",
    "decode_status": "Recovered",
    "integrity_score": 100.0
  }
]
```

---

## Alert Endpoints

### List All Alerts
**GET** `/alerts`

Retrieve all recovery alerts.

**Query Parameters:**
- `limit` (integer): Max records to return (default: 50)

**Response:** `200 OK`
```json
[
  {
    "id": 1,
    "patient_id": "PAT-005-FRAG",
    "title": "Critical BPM Elevation",
    "description": "Patient PAT-005 vital signs show critically elevated heart rate (145+ BPM)",
    "severity": "Critical",
    "category": "TelemetryAlert",
    "is_read": false,
    "created_at": "2024-03-26T10:30:00Z"
  },
  {
    "id": 2,
    "patient_id": null,
    "title": "Dataset Integrity Alert",
    "description": "2 patient records show corrupted checksum flags",
    "severity": "Warning",
    "category": "IntegrityFail",
    "is_read": false,
    "created_at": "2024-03-26T10:25:00Z"
  }
]
```

### Mark Alert as Read
**GET** `/alerts/{alert_id}/read`

Mark a specific alert as read.

**Path Parameters:**
- `alert_id` (integer): Alert ID

**Response:** `200 OK`
```json
{
  "id": 1,
  "patient_id": "PAT-005-FRAG",
  "title": "Critical BPM Elevation",
  "description": "Patient PAT-005 vital signs show critically elevated heart rate",
  "severity": "Critical",
  "category": "TelemetryAlert",
  "is_read": true,
  "created_at": "2024-03-26T10:30:00Z"
}
```

### Get Unread Alert Count
**GET** `/alerts/unread/count`

Get count of unread alerts.

**Response:** `200 OK`
```json
{
  "unread_count": 3
}
```

---

## Timeline Endpoints

### Get Incident Events
**GET** `/timeline`

Retrieve incident timeline events.

**Query Parameters:**
- `limit` (integer): Max records to return (default: 100)

**Response:** `200 OK`
```json
[
  {
    "id": 1,
    "title": "Recovery Engine Initialized",
    "description": "Forensic recovery platform initiated after ransomware incident detected",
    "severity": "Critical",
    "event_type": "DiskSnapshot",
    "created_at": "2024-03-26T08:00:00Z"
  },
  {
    "id": 2,
    "title": "Record Fragment Recovered",
    "description": "Successfully recovered 12 complete patient demographic records",
    "severity": "Info",
    "event_type": "RecordFragment",
    "created_at": "2024-03-26T08:15:00Z"
  }
]
```

---

## Dashboard Endpoints

### Get Dashboard Summary
**GET** `/dashboard/summary`

Retrieve KPI summary data for dashboard.

**Response:** `200 OK`
```json
{
  "total_patients_recovered": 12,
  "identity_reconstruction_confidence": 78.33,
  "critical_telemetry_alerts": 2,
  "medication_decode_success_rate": 82.35,
  "active_recovery_sessions": 3,
  "dataset_integrity_score": 84.17,
  "unread_alerts_count": 5,
  "critical_patients_count": 2
}
```

### Get User Profile
**GET** `/dashboard/profile`

Get current user's profile information.

**Response:** `200 OK`
```json
{
  "id": 2,
  "full_name": "James Patterson",
  "email": "analyst@lazarus.health",
  "role": "analyst",
  "created_at": "2024-03-26T10:30:00Z"
}
```

---

## Error Responses

### 401 Unauthorized
```json
{
  "detail": "Invalid token"
}
```

### 404 Not Found
```json
{
  "detail": "Patient not found"
}
```

### 422 Validation Error
```json
{
  "detail": [
    {
      "loc": ["body", "email"],
      "msg": "invalid email format",
      "type": "value_error.email"
    }
  ]
}
```

---

## Status Codes

| Code | Meaning |
|------|---------|
| 200 | OK - Request successful |
| 400 | Bad Request - Invalid input |
| 401 | Unauthorized - Auth required |
| 404 | Not Found - Resource missing |
| 422 | Validation Error - Invalid data |
| 500 | Server Error |

---

## Data Enumerations

### Patient Reconstruction Status
- `Stable` - Patient data recovered with high confidence
- `Warning` - Some data issues, but mostly recovered
- `Critical` - Significant data corruption or integrity issues

### Medication Decode Status
- `Recovered` - Medication fully decoded and verified
- `Partial` - Medication partially decoded
- `Corrupted` - Medication data corrupted
- `Missing` - Medication record not found

### Alert Severity
- `Critical` - Immediate action required
- `Warning` - Prompt review recommended
- `Info` - Status/informational

### Alert Categories
- `TelemetryAlert` - Vital signs abnormality
- `IdentityMismatch` - Patient identification conflict
- `MedicationDecodeFail` - Medication decoding failure
- `IntegrityFail` - Data integrity issue
- `CrossDatasetConflict` - Conflict between datasets

### User Roles
- `admin` - Full access to all features
- `analyst` - Access to recovery analysis and patient data
- `clinician` - Access to patient clinical data

### Checksum Status
- `valid` - Checksum validation passed
- `warning` - Checksum shows minor issues
- `corrupted` - Checksum validation failed

---

## Rate Limiting

No rate limiting on local development. Production deployment should implement rate limiting via:
- API Gateway
- Reverse Proxy (nginx)
- FastAPI middleware

---

## Example cURL Requests

### Login
```bash
curl -X POST "http://localhost:8000/api/auth/login" \
  -H "Content-Type: application/json" \
  -d '{
    "email": "analyst@lazarus.health",
    "password": "analyst123"
  }'
```

### Get Patients
```bash
curl -X GET "http://localhost:8000/api/patients" \
  -H "Authorization: Bearer <token>"
```

### Get Patient Details
```bash
curl -X GET "http://localhost:8000/api/patients/PAT-001-FRAG" \
  -H "Authorization: Bearer <token>"
```

### Get Telemetry
```bash
curl -X GET "http://localhost:8000/api/telemetry/PAT-001-FRAG?limit=10" \
  -H "Authorization: Bearer <token>"
```

---

## Integration Examples

### JavaScript/Fetch
```javascript
const token = localStorage.getItem('token');

// Get patients
fetch('http://localhost:8000/api/patients', {
  headers: {
    'Authorization': `Bearer ${token}`
  }
})
.then(r => r.json())
.then(data => console.log(data));
```

### Python/Requests
```python
import requests

token = 'your_jwt_token'
headers = {'Authorization': f'Bearer {token}'}

# Get patients
response = requests.get(
    'http://localhost:8000/api/patients',
    headers=headers
)
print(response.json())
```

---

## API Documentation

Full interactive API documentation available at:
- **Swagger UI:** http://localhost:8000/docs
- **ReDoc:** http://localhost:8000/redoc

---

**Last Updated:** March 2024
**API Version:** 1.0
**Status:** Stable
