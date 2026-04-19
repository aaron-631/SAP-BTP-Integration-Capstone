# SAP BTP Integration Suite — Employee Data Integration
### Capstone Project | SAP Industrial Elective — BTP Developer | KIIT University

---

## 👤 Student Details

| Field | Details |
|---|---|
| **Name** | Aaron Chakraborty |
| **Roll Number** | 2305671 |
| **Email** | 2305671@kiit.ac.in |
| **Program** | SAP Industrial Elective — BTP Developer |
| **University** | KIIT University, Bhubaneswar |
| **Batch** | 2026 |

---

## 📋 Project Overview

This project implements an **end-to-end SAP BTP Integration Suite** scenario aligned with the **Hire-to-Retire (H2R)** SAP business process.

An Integration Flow (iFlow) named `Employee_Data_Fetch` is designed, configured, and deployed on SAP BTP Cloud Integration. The iFlow:
- Exposes a secure **HTTPS endpoint** to receive inbound integration requests
- Uses a **Request-Reply** step to call an external REST API
- Fetches **real-time employee data** (GET /users/1 from JSONPlaceholder)
- Returns the structured **JSON response** to the calling system — fully automated

---

## 🏗️ Architecture

```
[External System]
       |
       | HTTP POST → /employeedata
       ↓
┌─────────────────────────────────────────┐
│         SAP BTP Integration Suite       │
│                                         │
│  [Sender(HTTPS)] → [Start]              │
│        → [Request Reply]                │
│        → [End] → [Receiver(HTTP)]       │
│                                         │
│  Package: KIIT_BTP_Capstone             │
│  iFlow:   Employee_Data_Fetch           │
└─────────────────────────────────────────┘
       |
       | GET https://jsonplaceholder.typicode.com/users/1
       ↓
[JSONPlaceholder REST API → Employee JSON Response]
```

---

## 🛠️ Technology Stack

| Technology | Version | Role |
|---|---|---|
| SAP BTP | Trial | Cloud platform |
| SAP Integration Suite | Trial Plan | Integration middleware |
| Cloud Integration | Trial Tenant | iFlow design & runtime |
| HTTPS Adapter | Built-in | Inbound trigger |
| HTTP Adapter | Built-in | Outbound REST call |
| JSONPlaceholder API | Public REST | External employee data source |
| SAP BTP Cockpit | Web Console | Account & service management |

---

## 📁 Repository Structure

```
SAP-BTP-Integration-Capstone/
│
├── README.md                          ← This file
├── Aaron_Chakraborty_SAP_BTP_Capstone.pdf  ← Project documentation PDF
│
└── screenshots/
    ├── 01_btp_cockpit.png             ← SAP BTP Trial Cockpit
    ├── 02_integration_suite.png       ← Integration Suite capabilities
    ├── 03_roles_assigned.png          ← Role collections assigned
    ├── 04_iflow_canvas.png            ← iFlow design canvas
    └── 05_monitor_deployment.png      ← Monitor deployment screen
```

---

## 🚀 Setup & Deployment Steps

### Prerequisites
- SAP BTP Trial account (free at sap.com/trial)
- Integration Suite subscribed with Cloud Integration capability activated
- Role collections assigned: `Integration_Provisioner`, `PI_Administrator`, `PI_Integration_Developer`

### Step 1 — Create Integration Package
1. Open Integration Suite → Design → Integrations and APIs
2. Click **Create** → Fill Name: `KIIT_BTP_Capstone`
3. Click **Save**

### Step 2 — Create iFlow
1. Inside the package → Click **Edit** → **Add** → **Integration Flow**
2. Name: `Employee_Data_Fetch`
3. Click **OK** to open the canvas

### Step 3 — Configure HTTPS Sender
1. Hover over Sender → drag arrow to Start → select **HTTPS** adapter
2. Click Connection tab → Address: `/employeedata`
3. Uncheck CSRF Protected → Save

### Step 4 — Add Request Reply Step
1. Search "Request Reply" in step search → place between Start and End
2. Connect Request Reply → Receiver → select **HTTP** adapter
3. Connection tab settings:
   - Address: `https://jsonplaceholder.typicode.com/users/1`
   - Method: `GET`
   - Authentication: `None`

### Step 5 — Deploy
1. Click **Save** → Click **Deploy**
2. Monitor → All Integration Flows → wait for status: **Started**

---

## 📊 Sample API Response

```json
{
  "id": 1,
  "name": "Leanne Graham",
  "username": "Bret",
  "email": "Sincere@april.biz",
  "address": {
    "street": "Kulas Light",
    "city": "Gwenborough"
  },
  "phone": "1-770-736-0988 x56442",
  "company": {
    "name": "Romaguera-Crona"
  }
}
```

---

## 🔮 Future Improvements

1. **OAuth 2.0 Security** — Secure the HTTPS endpoint with client credentials
2. **Message Mapping** — Transform JSON to SAP IDoc/XML format
3. **S/4HANA Connection** — Connect to real SAP S/4HANA HR OData service
4. **Error Handling** — Exception subprocesses with email alerts
5. **Audit Logging** — Trace-level logging and monitoring dashboards
6. **Bulk Processing** — Paginated API calls for all employees

---

## 📄 Documentation

Full project documentation PDF is included in this repository:
`Aaron_Chakraborty_SAP_BTP_Capstone.pdf`

---

*Submitted as part of KIIT University Capstone Project — ExcelR Edtech SAP Industrial Elective, April 2026*
